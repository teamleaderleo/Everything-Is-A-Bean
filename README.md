# Everything Is A Bean

> A curated collection of real-world projects proving that, given sufficient confidence and a Maven dependency, any problem can become a Spring bean.

Spring is excellent software. This repository is about what happens when that fact is interpreted as a challenge.

This is a museum of **real, source-verifiable projects** where Spring or Spring Boot appears somewhere delightfully unexpected: robot drivetrains, FUSE filesystems, software-defined radio, CAN bus drivers, G-code motion control, GPU compute, telescope mounts, CPU emulators, 3D printers, stage lighting, firmware flashing, and other places where one expects to encounter `malloc`, an interrupt, or a datasheet before encountering `ApplicationContext`.

No shame is intended toward the authors. Many of these projects are clever, useful, educational, or completely reasonable in context. We are here because the juxtaposition is magnificent.

## The governing law

```text
Can Java access it?
        |
        v
Can it conceivably be represented as an object?
        |
        v
@Component
```

## Hall of fame

| Curse | Project | Category | The offense |
|---|---|---|---|
| 🫘🫘🫘🫘🫘 | [team3316/frc-2020](https://github.com/team3316/frc-2020) | Robotics | Physical drivetrain motor controllers, pneumatics, and a gyro are declared as Spring XML beans on an FRC robot. |
| 🫘🫘🫘🫘🫘 | [EGA-archive/ega-fuse-client](https://github.com/EGA-archive/ega-fuse-client) | Filesystem | A Spring Boot application creates a FUSE filesystem as a bean; the filesystem implements `readdir`, `getattr`, `read`, `open`, `unlink`, mount, and unmount. |
| 🫘🫘🫘🫘🫘 | [tauasa/rtlsdr-fx](https://github.com/tauasa/rtlsdr-fx) | SDR / DSP | A Spring `@Service` owns the RTL-SDR signal path and processes incoming IQ blocks, demodulates audio, and computes spectrum frames. |
| 🫘🫘🫘🫘🫘 | [pnoker/iot-dc3](https://github.com/pnoker/iot-dc3) | CAN / industrial I/O | Contains an actual `@SpringBootApplication` named `CanDriverApplication` and a Spring `@Service` that sends and receives CAN frames. |
| 🫘🫘🫘🫘🫘 | [fablab-fribourg/EggBot](https://github.com/fablab-fribourg/EggBot) | Motion control | A Spring `@Service` owns a G-code sender thread and uses an `@Autowired SerialService` to feed physical motion commands to a controller. |
| 🫘🫘🫘🫘🫘 | [jorjazo/nocs](https://github.com/jorjazo/nocs) | Astronomy | Spring Boot observatory control: discover a telescope mount, slew to RA/DEC, sync, park, unpark, abort, and emergency-stop it. |
| 🫘🫘🫘🫘 | [xmaiax-org/brutalcraft](https://github.com/xmaiax-org/brutalcraft) | Graphics engine | A Java + LWJGL3 + Spring Boot game engine where the OpenGL `Renderer2D` is a Spring component. |
| 🫘🫘🫘🫘 | [EduardoSaverin/KeyDB](https://github.com/EduardoSaverin/KeyDB) | Storage engine | The actual Bitcask-inspired storage engine is a Spring `@Component`: append logs, CRCs, fsyncs, hint files, recovery, tombstones, rotation, and compaction. |
| 🫘🫘🫘🫘 | [IkhwanAL/cpu-simulation](https://github.com/IkhwanAL/cpu-simulation) | CPU emulator | Spring MVC accepts a program, builds emulated CPU + RAM, then ticks the CPU until the ALU halts and renders the registers back to the user. |
| 🫘🫘🫘🫘 | [yongzhegege/kvm-console](https://github.com/yongzhegege/kvm-console) | Virtualization | Spring services drive libvirt/QEMU/KVM, create qcow2 disks, define domains, attach devices, and call `domain.create()`. |
| 🫘🫘🫘🫘 | [RePro3D-Praxisproject/repro3d-backend](https://github.com/RePro3D-Praxisproject/repro3d-backend) | 3D printing | A Spring Boot `PrinterService` talks to OctoPrint, starts physical print jobs, polls completion, and proxies printer webcam video. |
| 🫘🫘🫘🫘 | [Rocket-Show/rocketshow](https://github.com/Rocket-Show/rocketshow) | Show control | Spring Boot on Raspberry Pi runs audio, video, MIDI, and DMX stage lighting. Yes, `DefaultMidi2LightingConvertService` exists. |
| 🫘🫘🫘🫘 | [rucko24/EspFlow](https://github.com/rucko24/EspFlow) | Firmware | Spring Boot tooling for ESP32/ESP8266 devices; an `@Service` enumerates devices and invokes flash read/write operations through `esptool`. |
| 🫘🫘🫘 | [NextGPUNetwork/nextgpu-app](https://github.com/NextGPUNetwork/nextgpu-app) | Audio I/O | A Spring `@Service` directly opens a microphone `TargetDataLine`, reads PCM buffers on its own thread, and computes live amplitude. |
| 🫘🫘🫘 | [ANnianExplorer/aiToVoice](https://github.com/ANnianExplorer/aiToVoice) | Audio DSP | A Spring `@Service` runs TarsosDSP/YIN pitch detection and computes pitch statistics from audio. |
| 🫘🫘🫘 | [tckb/busylight_rest](https://github.com/tckb/busylight_rest) | USB hardware | A Spring Boot REST interface wraps a physical USB Busylight driver so HTTP can make a desk lamp change color or tone. |

The full evidence trail lives in [CATALOG.md](CATALOG.md). A machine-readable version lives in [`projects.yml`](projects.yml).

## Curse scale

This is deeply scientific.

- **🫘 — Ordinary enterprise behavior.** Spring is present; nobody has yet offended the machine spirits.
- **🫘🫘 — Unusual habitat.** Desktop, CLI, game, embedded-ish, or hardware-adjacent work.
- **🫘🫘🫘 — Direct device or compute involvement.** Serial ports, microphones, cameras, GPU compute, packet capture, etc.
- **🫘🫘🫘🫘 — Spring owns a low-level subsystem.** Renderer, storage engine, VM manager, firmware flasher, printer controller.
- **🫘🫘🫘🫘🫘 — The bean can move matter or impersonate an OS primitive.** Motor controllers, machine motion, radio hot paths, CAN drivers, filesystems, telescope motion.

Bonus points are awarded for:

- physical actuators represented as beans;
- `@Autowired` within a latency-sensitive loop;
- classes named `*DriverApplication`;
- Spring lifecycle annotations controlling hardware lifecycle;
- `application.yml` settings that influence something with voltage;
- JNI added specifically to move Spring closer to the metal;
- XML bean definitions for objects that can hurt you.

## What counts

A submission should have **source evidence**, not merely a README claiming Spring is somewhere in the stack.

Strong evidence includes:

1. a Spring annotation or application context in the relevant code path;
2. a direct link to the low-level/hardware subsystem;
3. enough source to connect the two without guesswork.

The ideal specimen lets us write a sentence like:

> The Spring-created object receives IQ samples from an SDR and demodulates them.

or:

> The Spring bean sends the G-code that moves the machine axis.

or, in the highest form:

> The physical drivetrain motor controller is declared in Spring XML.

## What does not count

A normal REST service that happens to call a database is just Tuesday.

Spring at the outer API boundary of an otherwise unrelated native program can still be funny, but it scores lower. We care most about cases where Spring crosses into the unusual subsystem itself.

## Contributing

Found something worse? Excellent.

Read [CONTRIBUTING.md](CONTRIBUTING.md) and submit the source links. The standard of proof is simple: **show us the bean and show us the crime.**

## The final bosses

We are still hunting credible examples of:

- a JVM implementation whose interpreter / heap / class loader / GC are Spring-managed;
- a real pre-OS bootloader using Spring somehow;
- a kernel-mode device driver whose design genuinely involves Spring on the control path;
- a database query executor, buffer pool, or WAL implementation wired primarily through Spring;
- anything that causes the sentence “the actuator could not initialize because one of its dependencies could not be injected.”

If these exist, they belong here.

---

**Everything Is A Bean**: because `new` was apparently too personal.