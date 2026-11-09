# AME / D10: Project Overview and Technical History

Language: [简体中文](HISTORY.zh-CN.md) | **English**

If this were actually a short introduction, the words “Technical History” would not be in the title.

If you do not care about the archaeology below, here is the one-sentence version:

**I originally just wanted six-axis motion controls on an EasySMX D10 under Windows. Somewhere along the way, I ended up digging through the controller, receiver, wireless protocol, motion stack, calibration, driver switching, input mapping, profiles, macros, diagnostics, and an entire engineering validation system.**

Great. You have finished the document. You may close this tab now. :)

Still here?

Fine. From the beginning.

## 1. May the Anemo Archon Bamboozle You

The D10 started out as a birthday present to myself. The reasons were simple: it looked good, fit larger hands, seemed to have motion controls, and a used one was cheap.

My plan was to use the controller itself as a steering wheel for racing and driving games. Before it even arrived, I discovered one tiny detail: yes, the D10 has six-axis motion hardware. Officially, though, that feature lives in Switch mode. Apparently I had bought the educational edition of controller support. (Moral of the story: read the product page properly.)

Then, during some highly productive doomscrolling on Bilibili, I found someone using Switch mode over Bluetooth to get motion input working on PC.

So I thought: maybe, just maybe, all I need is a Bluetooth adapter and a small detour. How hard could that be?

Reality had notes.

The triggers are physically analog, but once the controller enters NS mode, ZL and ZR become digital. For driving, that gives you two pedal positions: **floor it** and **emergency stop**.

At this point my inner Ork kicked in.

Maybe, just maybe, I could write a driver myself.

How hard could that be?

May the Anemo Archon bamboozle you.

## 2. And So It Began

In July 2026, the earliest D10 project used a KMDF / IOCTL kernel-driver architecture. Development happened with Windows Test Mode enabled. During P0.1 through P0.7a, the first-generation motion steering stack also took shape: complementary fusion, bad-sample protection, VelocityLead, TrajectoryGate, VibrationGuard, and the final stick mapping were all already there.

Technically, that path could have continued. The problem was distribution.

Test Mode is fine when I am the only person using the thing. Asking every future user to enable it just to use a controller is less fine. A proper kernel driver, meanwhile, meant signing, certificates, and certification.

My ambitions were rich.

My wallet was not.

So the question changed from “how do I keep developing this kernel driver?” to:

**Can I just throw the kernel driver overboard?**

On July 17, P0U became the main line. The D10 moved to WinUSB: EP81 for normal input, EP83 for motion, EP02 for rumble. Processing moved into user space, and ViGEm presented the final result to games as a virtual Xbox 360 controller.

The existing Motion work did not get thrown away. What changed was the device-access and runtime architecture underneath it.

There was an obvious cost: switching drivers became more annoying.

Looking back, it was worth it. Once 2.4 GHz support, M buttons, input mapping, and macros started showing up, the user-mode architecture saved a considerable amount of pain.

Getting 2.4 GHz working, however, required another archaeological expedition.

## 3. 2.4 GHz: Digging Up the Receiver

Once wired mode worked, wireless was the obvious next target.

My original assumption was simple. Wired motion lived on EP83, so surely the 2.4 GHz path had not magically eaten the IMU. The official PC path probably just did not expose it.

Never trust a bard.

I tested it.

Did not believe the result.

Tested it again.

Eventually accepted defeat.

There was no wireless equivalent of EP83.

Fine. Time to go deeper.

Ghidra came out, followed by receiver firmware archaeology. There was even a small side quest where the firmware turned out to be encrypted. I originally planned to look for the key.

Brute-force lockpicking was faster.

On July 28, the first modified receiver firmware with motion support, v0.5ad, was born. Normal XInput traffic and rumble continued through the official path, while motion was extracted separately through the receiver's private `C0/5A` path.

That solved Motion.

Then another question showed up:

Where are M, M1, and M2?

Out came the Luoyang shovel again.

On July 29, v0.5ae kept `C0/5A` for motion and added `C0/5C`, which mirrored part of the raw RF traffic seen by the receiver.

The logic was not sophisticated:

**If I cannot find where the data is hiding, make the receiver dump what it actually sees and search the pile.**

By August 1, the basic wireless P0U path was complete:

```text
EP81   → buttons / sticks / triggers
C0/5A  → six-axis motion
C0/5C  → M / M1 / M2
EP02   → rumble
```

An earlier `xusb22 + filter` route did not survive as the product design. Wireless eventually joined wired mode on the same general architecture:

```text
whole-device WinUSB → P0U → ViGEm
```

At that point, 2.4 GHz finally had nearly the same practical capability as wired mode.

Then came the next question.

How fast was any of this actually running?

## 4. 200 Hz, Except Not Quite 200 Hz

Wired and wireless turned out to be very different here.

Later measurements put wired EP83 close to 500 Hz. Motion and M/M1/M2 coexist in the same reports, so nobody has to fight over the same polling budget.

Wireless was poorer.

`C0/5A` Motion and `C0/5C` M-button traffic share the same private EP0 budget. The measured ceiling was about 333.333... Hz. The product did not run right against that ceiling; it settled on 330 Hz.

How to divide those 330 slots changed several times. There was 205 / 125, then 200 / 130, and for a while I even had a build where the user could adjust the split.

Eventually the answer got simpler. Motion should get as much bandwidth as possible, but starving M/M1/M2 just to squeeze out a few more motion samples would be silly.

The final schedule became 3:2:

```text
Motion - M - Motion - M - Motion
```

Five slots per cycle.

330 / 5 gives 66 Hz per slot. Motion gets three slots, or **198 Hz**. M buttons get two, or **132 Hz**.

198 / 132 looks slightly odd until you see the schedule. Then it is probably the neatest split the wireless path ever had.

And then testing found something much stranger.

The source really was delivering about 200 Hz.

The fusion engine was getting a little over 60.

The receiver was innocent this time.

The program had done it to itself.

## 5. 200 Hz In, 67 Hz Out

On August 4, 2026, one hardware diagnostic captured 5,091 wireless motion samples at an average of **199.968 Hz**, almost exactly one sample every 5 ms.

Only 1,497 made it into fusion.

**67.088 Hz.**

The receiver had done its job. The software consumed roughly 29.4% of what it was given.

The data path looked like this:

```text
new C0/5A sample
    ↓
store only "latest sample"
    ↓
MotionLoop wakes periodically
    ↓
consume whatever is latest at that moment
```

The device produced a new frame every 5 ms. The fusion thread was nominally polling every 10 ms, but under the Windows scheduling behavior I was seeing at the time, it often slept for around 15.6 ms.

In English-internet terms, the runtime was basically doing this:

```text
Frame 1 arrives   (I sleep)
Frame 2 arrives   (I sleep)
Frame 3 arrives   (REAL SHIT?)
Thread wakes up: oh, Frame 3.
Back to sleep.
```

The first two frames had already been overwritten by the newest one.

In that capture, **2,965 source samples** disappeared this way. The most common wake-up pattern also happened to skip across three source sequence numbers at a time.

So wireless motion had never really dropped to 67 Hz.

The software had eaten 200 Hz and produced 67 Hz.

On August 5, Final1n-Fix1a replaced that mailbox-style path with a FIFO:

```text
new C0/5A sample
    ↓
FIFO queue
    ↓
AutoResetEvent wake-up
    ↓
dedicated fusion thread
    ↓
consume every sample in order
```

Samples now queued as they arrived. If Windows decided the thread needed an unscheduled nap, the thread simply had more work waiting when it woke up. Intermediate history no longer vanished.

Per-frame timing also continued to use each sample's own `SourceQpc` timestamp instead of pretending the scheduler wake-up time was the sensor timestamp.

Wireless got the fix first.

Then I noticed wired mode was still doing the old thing.

P0.8 moved wired motion onto the same FIFO + event-driven architecture.

Around this point, the thing I had been calling “the motion steering algorithm” also started developing cleaner boundaries.

That is where AME began to emerge.

## 6. AME: The Controller Started Playing Too

Once all those high-rate samples were actually reaching the algorithm, another problem became much harder to ignore:

**the controller itself was moving the controller.**

More specifically, the rumble motors inject periodic vibration into the gyroscope.

Hit something in a game, drive over a curb, or trigger a long rumble effect, and the sensor sees both the player's real movement and the controller buzzing itself.

Earlier builds already had protections such as VibrationGuard. P0.8 started treating motor interference as its own problem, and Beta1 introduced `AdaptiveMotorInterferenceCanceller`.

The easy solution would have been:

> rumble on, gyro clocks out.

That would also be terrible.

Instead, the canceller tries to identify periodic motor interference in the gyro signal and subtract it. The production implementation mainly watches roughly the 18–45 Hz range and can track two dominant interference tones at once.

Unfortunately, humans also move the controller.

If the model gets too enthusiastic and learns the player's steering input as “noise,” we have invented an extremely advanced system for deleting the user.

So it also keeps a slower reference for human motion. When obvious hand movement is present, interference learning pauses rather than absorbing that motion into the cancellation model.

By this point, the code was starting to look less like one giant “steering algorithm.” A growing amount of it was simply trying to turn raw motion into trustworthy motion before anybody decided what that motion should control.

Beta2 moved that generic preprocessing out of `SteeringFusionEngine` into a separate layer:

`AdaptiveMotionEngine`

Or **AME**, which is what I usually call it now.

(Ahem. Absolutely not because I never remember the full name.)

The rough split became:

```text
AdaptiveMotionEngine
    ↓
clean up / validate raw Motion

SteeringFusionEngine
    ↓
interpret processed Motion as steering
```

AME did not suddenly appear from nowhere in August. TrajectoryGate, VibrationGuard, dt handling, smoothing, and other pieces had already been growing throughout the July P0.x builds.

Beta2 mostly gave them a proper home.

On August 9, wireless contributed one more small trap. Two consecutive identical gyro tuples do not necessarily mean the sensor independently produced the same observation twice. Sometimes the RF side has simply repeated the same data.

FreshnessGuard was added around that point.

Same value, same packet history: not automatically another vote.

By then, the early AME core was largely in place.

The surrounding project changed violently over the following months.

This layer, oddly enough, mostly did not.

## 7. October: I Tried to Slack Off. NTE Had Other Plans.

After the July and August excavation marathon, October was mostly supposed to be a period where, if both the code and I were still running, neither of us should be disturbed unnecessarily.

My version of “taking a break” was mostly playing games in the name of product testing.

Sometimes that worked.

Sometimes I found a bug and accidentally deleted the next several days of my break.

That is how **Neverness to Everness (NTE)** entered the story.

The first time I tried driving with motion steering, something felt wrong. I would steer, let go, and sometimes the car would not settle back cleanly. It could keep drifting to one side.

My first reaction was not “the game is broken.”

It was:

Great. I broke the driver again.

So I tested.

Changed things.

Tested again.

At the time I was also working around 3D Fusion, centering, and deadzone behavior, so naturally all suspicion started on my side.

After one round of that, I searched the community.

That made things awkward.

The developers had already acknowledged that vehicle handling and control feel still needed work, and later Version 1.1 notes included more vehicle-handling changes plus improved controller responsiveness and feel. Players had their own reports: steering that felt unusually sensitive only in NTE, vehicles swerving with multiple controllers, and cases where increasing the Vehicle Joystick Deadzone reduced the problem.

Interesting.

Still not proof that my driver was innocent.

So I went back and kept debugging.

I tested fixed LX values. Captured real output logs. Eventually I wrote an `XInput Readback Probe` to check what Windows was actually seeing at the end of the virtual-controller path.

Then I searched again.

The developers really were still changing this stuff.

Players really were still reporting it.

By this point, the game was looking increasingly suspicious, but there was still one embarrassingly simple experiment missing.

We had spent all this time testing motion steering.

We had never seriously tried reproducing it with the physical left stick.

So I disabled motion.

AME was out.

The gyro was out.

I nudged the D10's physical left stick slightly left and slowly released it back to mechanical center.

Pop.

Same drift.

Straight into the next ZIP code.

That single test changed the whole investigation. At minimum, the NTE behavior could no longer be used as direct evidence that the Motion stack was at fault.

The previous debugging was not wasted. IMU, AME, ViGEm, and XInput had all been dragged into the interrogation room, and by the end the boundaries were much clearer.

These days, when something odd happens on real hardware, changing the algorithm is usually not step one.

First figure out whose problem it actually is.

Otherwise you end up with a very efficient development process:

**Game's sick. Driver gets the medicine.**

## 8. The Rest of October

NTE could mostly be put aside after that. The motion stack still had real problems of its own.

Early motion steering mostly assumed that the controller stayed reasonably level. Full six-axis use is less polite. Pitch, Roll, and Yaw can all change together, and the sensor coordinate frame rotates with the controller.

At that point, two questions that had once looked like one started pulling apart:

What is the controller's full 3D attitude?

And how much of that attitude should count as steering?

At the end of September, I added a 15-pose three-axis hardware calibration process to refit accelerometer offsets and scale. Before trusting the ruler, it seemed sensible to make sure the ruler itself was not bent.

S4.1 then went through several approaches to 3D steering. Some adjusted full attitude in pursuit of a cleaner center. Some worked with quaternion gauge. One route tried integrating an independent SteeringPhase.

Several of them looked promising in limited tests and then fell apart on hardware or under larger attitudes.

The useful lesson that survived all those graves was simple:

**Full 3D attitude is not the same thing as steering angle.**

S4.2 A0, on October 31, took a different approach. The 3D observer continued estimating full attitude, while steering became a separate observable derived from the difference between fused gravity direction and the saved calibration reference.

That approach survived.

REV3, REV5, REV6, and their relatives are still buried in the internal archaeology packs. No need to give every grave its own guided tour here.

October was supposed to be my month off.

Instead I got accelerometer calibration, quaternions, 3D observables, game-input debugging, and several algorithm branches that I personally got to bury.

I was not very good at taking October off.

## 9. November: Cleaning Up the House

By November, Motion itself no longer needed to be touched every day.

The remaining work started looking much more like normal software development.

Which, in many cases, was worse.

Center calibration had to survive restarts instead of asking the user to leave the controller still for a few seconds every time. The 15-pose hardware calibration needed to become a real user workflow rather than a research tool. Wired and 2.4 GHz could not each invent their own rules. Profiles, automatic switching, input mapping, keyboard and mouse output, macros, M1/M2, diagnostics, and recovery all needed proper state and failure behavior.

WinUSB also carried a debt dating back to P0U: users have to switch the D10 between the official driver path and this one.

At first, that meant scripts.

Then there were more scripts.

Then scripts became something that could ruin your evening.

Eventually driver switching, rollback, and Emergency recovery became proper product flows.

At this point, “how do I read one IMU frame?” was no longer the scary question.

“How do I make sure the user's controller is still recoverable if a driver transition dies halfway through?” was much more interesting.

The test system grew the same way.

A few scripts became build checks, identity checks, persistence checks, diagnostics, driver-transition tests, and regression gates.

Then the gate system got too large.

So I spent time making that smaller again too.

The most useful thing Gate ever taught me is probably this:

**All green means you answered every question in the test correctly. If the test itself is wrong, you can still score 100%.**

REV5 passed the automated engineering chain and still lost to real hardware.

After that, “engineering PASS” and “the product is physically correct” stopped being interchangeable sentences.

The receiver firmware also needed a sane public-distribution story. Shipping modified copies of official firmware directly was not a good answer, so I built a small Firmware Patcher instead. The user selects the official firmware, the tool validates it, and then produces the patched version locally.

By this point, calling the project “a PC motion driver for the D10” was becoming increasingly inaccurate.

## 10. So What Is This Thing Now?

Looking at it today, there are a few reasonably clean layers.

The D10-specific device side handles WinUSB, USB / RF protocol work, receiver firmware, normal controller input, rumble, and device identity.

AME handles Motion preprocessing: timing, suspicious samples, trajectory evidence, motor interference, and the other dirty work required before the data is trustworthy enough to use.

Steering / Consumer logic decides what to do with that Motion. The D10 currently uses it mainly for steering, but there is no architectural reason it has to stop there.

Outside of that is the actual product users interact with: Profiles, mappings, macros, automatic switching, calibration, diagnostics, driver transitions, and the GUI.

Those boundaries did not exist cleanly at the beginning. Everything used to live inside the vague problem called “make the controller drive the car.”

At least now everybody has a name and somewhere to live.

The project will continue being cleaned up for public release. There is no reason to dump months of internal archives, failed experiments, Gate graveyards, and archaeological evidence wholesale onto GitHub.

The public project needs the code, architecture, technical reasoning, known limitations, and enough history to explain why the weird parts are weird.

The original question was answered a long time ago:

Can the D10 use six-axis motion controls on PC?

Yes.

I may have over-researched the answer just a little.

## Sources

The NTE section refers to public developer notes and player reports, including:

- Official NTE developer communications acknowledging that vehicle handling and control feel still needed improvement.
- The official Version 1.1 “Dreamwalk Corridor” update notes, which included vehicle-handling changes and improvements to controller responsiveness and feel.
- Later official patch notes continuing to mention controller-support improvements.
- Community reports describing vehicle swerving, suspected stick drift that appeared only in NTE, reproduction across multiple controllers, and cases where increasing Vehicle Joystick Deadzone reduced the symptom.

These references are included only to show that similar symptoms were reported independently. They are **not** evidence for which internal NTE subsystem caused the behavior.
