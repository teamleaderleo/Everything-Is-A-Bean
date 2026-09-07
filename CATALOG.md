# Catalog of Bean Crimes

This file is the evidence locker.

Each entry answers two questions:

1. **Where is Spring?**
2. **Where is the cursed subsystem?**

If those two things never actually meet, the project does not get the good beans.

---

## 🫘🫘🫘🫘🫘 team3316/frc-2020 — physical robot hardware as Spring XML beans

**Category:** robotics / motor control / pneumatics

**Repository:** https://github.com/team3316/frc-2020

**Why it is here:** the robot loads a Spring application context on the real FRC controller, and the deployed XML declares actual motor controllers, pneumatics, and inertial hardware as beans.

**Evidence:**

- Spring context loading on the robot: https://github.com/team3316/frc-2020/blob/415af8e2bc196ea4cf1ac8d9f7773f7a85349eb4/src/main/java/frc/robot/utils/Utils.java
- Deployed Spring bean configuration: https://github.com/team3316/frc-2020/blob/415af8e2bc196ea4cf1ac8d9f7773f7a85349eb4/src/main/deploy/DeployApllicationContext.xml

The XML contains bean definitions for drivetrain Talons/Victors, SparkMax controllers, `DoubleSolenoid`, and `PigeonIMU`.

**Signature crime:** the physical drivetrain controller is a Spring bean.

---

## 🫘🫘🫘🫘🫘 EGA-archive/ega-fuse-client — a FUSE filesystem created by Spring

**Category:** filesystem / OS integration

**Repository:** https://github.com/EGA-archive/ega-fuse-client

**Why it is here:** this is a Spring Boot application whose configuration creates the FUSE filesystem object as a bean. That object subclasses `FuseStubFS` and implements operations called by the operating system.

**Evidence:**

- Spring Boot entry point: https://github.com/EGA-archive/ega-fuse-client/blob/master/src/main/java/uk/ac/ebi/ega/egafuse/EgaFuseApplication.java
- Spring configuration creating `EgaFuse`: https://github.com/EGA-archive/ega-fuse-client/blob/master/src/main/java/uk/ac/ebi/ega/egafuse/config/EgaFuseApplicationConfig.java
- FUSE implementation: https://github.com/EGA-archive/ega-fuse-client/blob/master/src/main/java/uk/ac/ebi/ega/egafuse/service/EgaFuse.java

The filesystem implements `readdir`, `getattr`, `read`, `open`, `unlink`, mount, and unmount.

**Signature crime:** Linux asks for a file and eventually a Spring-created object answers.

---

## 🫘🫘🫘🫘🫘 tauasa/rtlsdr-fx — IQ samples enter a Spring service

**Category:** software-defined radio / DSP

**Repository:** https://github.com/tauasa/rtlsdr-fx

**Why it is here:** the Spring service is directly in the radio processing path. It receives incoming IQ blocks, demodulates audio, computes spectrum data, and controls tuner parameters.

**Evidence:**

- Spring Boot entry point: https://github.com/tauasa/rtlsdr-fx/blob/90d33706c42b68ff6778ba4256365791376ef84b/src/main/java/org/tauasa/apps/sdr/SdrApplication.java
- `@Service` signal coordinator: https://github.com/tauasa/rtlsdr-fx/blob/90d33706c42b68ff6778ba4256365791376ef84b/src/main/java/org/tauasa/apps/sdr/service/SdrService.java

The key boundary is `onBlock(float[] iq)`: every incoming block reaches a Spring-managed object before spectrum display and, when enabled, demodulated audio output.

**Signature crime:** electromagnetic radiation becomes dependency-injected business logic.

---

## 🫘🫘🫘🫘🫘 pnoker/iot-dc3 — `CanDriverApplication`

**Category:** CAN bus / industrial I/O

**Repository:** https://github.com/pnoker/iot-dc3

**Why it is here:** the repository contains an application literally named `CanDriverApplication`, annotated as a Spring Boot application, plus a Spring service implementing CAN read/write behavior on Linux.

**Evidence:**

- `CanDriverApplication`: https://github.com/pnoker/iot-dc3/blob/031deb4453ae6a18a8f5a2c87c398dccf1e5790f/dc3-driver/dc3-driver-can/src/main/java/io/github/pnoker/driver/CanDriverApplication.java
- CAN driver service: https://github.com/pnoker/iot-dc3/blob/031deb4453ae6a18a8f5a2c87c398dccf1e5790f/dc3-driver/dc3-driver-can/src/main/java/io/github/pnoker/driver/service/impl/CanDriverCustomServiceImpl.java

The current implementation shells out to `cansend` / `candump`. Its source explicitly notes native SocketCAN JNI as a future lower-latency path.

**Signature crime:** the phrase `@SpringBootApplication public class CanDriverApplication` exists in production-shaped source code.

---

## 🫘🫘🫘🫘🫘 fablab-fribourg/EggBot — G-code motion through `@Autowired SerialService`

**Category:** machine motion / G-code / serial control

**Repository:** https://github.com/fablab-fribourg/EggBot

**Why it is here:** Spring is not merely hosting a UI. A Spring `@Service` owns a sender thread that dequeues G-code commands, writes them to a serial controller, and waits for `ok` responses.

**Evidence:**

- Spring application context used by the JavaFX application: https://github.com/fablab-fribourg/EggBot/blob/f9445ad166d6367d6e3d36f6bfdbe542fa78fef1/GCodeSender/src/main/java/net/collaud/fablab/gcodesender/SpringFxmlLoader.java
- G-code service: https://github.com/fablab-fribourg/EggBot/blob/f9445ad166d6367d6e3d36f6bfdbe542fa78fef1/GCodeSender/src/main/java/net/collaud/fablab/gcodesender/gcode/GcodeService.java
- Serial service: https://github.com/fablab-fribourg/EggBot/blob/f9445ad166d6367d6e3d36f6bfdbe542fa78fef1/GCodeSender/src/main/java/net/collaud/fablab/gcodesender/serial/SerialService.java

The service generates commands for X/Y motion and servo position and sends them through an autowired serial service.

**Signature crime:** `@Autowired` can move the machine.

---

## 🫘🫘🫘🫘🫘 jorjazo/nocs — Spring Boot observatory control

**Category:** telescope / astronomy / motion control

**Repository:** https://github.com/jorjazo/nocs

**Why it is here:** this Spring Boot application creates the device service that discovers INDI devices. The telescope mount adapter can issue physical mount operations such as slew, sync, park, unpark, abort, and emergency stop.

**Evidence:**

- Spring Boot entry point: https://github.com/jorjazo/nocs/blob/0608fb068d0e52c52f4beb1c172d913c167aaed3/src/main/java/dev/nocs/NocsApplication.java
- Spring bean creation for the device service: https://github.com/jorjazo/nocs/blob/0608fb068d0e52c52f4beb1c172d913c167aaed3/src/main/java/dev/nocs/config/AppBeansConfig.java
- Telescope mount adapter: https://github.com/jorjazo/nocs/blob/0608fb068d0e52c52f4beb1c172d913c167aaed3/src/main/java/dev/nocs/device/adapter/IndiMountAdapter.java

**Signature crime:** the application context can tell a telescope to slew across the sky.

---

## 🫘🫘🫘🫘 xmaiax-org/brutalcraft — OpenGL renderer as a Spring component

**Category:** game engine / OpenGL

**Repository:** https://github.com/xmaiax-org/brutalcraft

**Why it is here:** the project describes itself as a Java + LWJGL3 + Spring Boot game engine. Rendering code is Spring-managed, including a `Renderer2D` component that performs direct OpenGL work.

**Evidence:**

- Repository: https://github.com/xmaiax-org/brutalcraft
- Renderer source: https://github.com/xmaiax-org/brutalcraft/blob/cedacdcde80a827efef22d5c668089b941ebcbb4/src/main/java/com/github/xmaiax/renderer/Renderer2D.java

**Signature crime:** `glDrawArrays(...)`, but enterprise.

---

## 🫘🫘🫘🫘 EduardoSaverin/KeyDB — a database storage engine as `@Component`

**Category:** storage engine / database internals

**Repository:** https://github.com/EduardoSaverin/KeyDB

**Why it is here:** this is not Spring wrapping PostgreSQL. The actual Bitcask-inspired engine is a Spring component and directly implements durable storage behavior.

**Evidence:**

- Repository description: https://github.com/EduardoSaverin/KeyDB
- Engine source: https://github.com/EduardoSaverin/KeyDB/blob/replication/src/main/java/com/sumit/keydb/keydb/engine/KeyDBEngine.java

The engine handles append-only segment files, CRC checks, in-memory key directories, hint files, crash recovery, syncs, tombstones, file rotation, and compaction.

**Signature crime:** database crash recovery has bean lifecycle.

---

## 🫘🫘🫘🫘 IkhwanAL/cpu-simulation — Spring MVC drives a CPU fetch/decode/execute loop

**Category:** CPU emulator

**Repository:** https://github.com/IkhwanAL/cpu-simulation

**Why it is here:** the application is Spring Boot. Its controller accepts a program over HTTP, creates emulated CPU and RAM, decodes the program, and ticks the CPU until the ALU halts.

**Evidence:**

- Spring Boot application: https://github.com/IkhwanAL/cpu-simulation/blob/master/src/main/java/org/cpu/sim/cpusim/CpuSimApplication.java
- Web controller / execution loop: https://github.com/IkhwanAL/cpu-simulation/blob/master/src/main/java/org/cpu/sim/cpusim/WebController.java

**Signature crime:** HTTP → Spring MVC → fetch/decode/execute → registers rendered into the model.

---

## 🫘🫘🫘🫘 yongzhegege/kvm-console — Spring services control libvirt/QEMU/KVM

**Category:** virtualization / hypervisor control

**Repository:** https://github.com/yongzhegege/kvm-console

**Why it is here:** Spring services talk directly to libvirt. They create qcow2 images, build domain XML, define virtual machines, attach devices, query QEMU guest state, and start domains.

**Evidence:**

- VM service: https://github.com/yongzhegege/kvm-console/blob/45ec31c08eec11aae6861d7b582bc9b7a7ebde86/src/main/java/com/example/demo/service/impl/VmServiceImpl.java
- Spring startup component monitoring libvirt domains: https://github.com/yongzhegege/kvm-console/blob/45ec31c08eec11aae6861d7b582bc9b7a7ebde86/src/main/java/com/example/demo/InitCommand.java

**Signature crime:** `@Service` eventually calls `domain.create()`.

---

## 🫘🫘🫘🫘 RePro3D-Praxisproject/repro3d-backend — Spring starts physical 3D prints

**Category:** 3D printing

**Repository:** https://github.com/RePro3D-Praxisproject/repro3d-backend

**Why it is here:** a Spring Boot printer service talks to OctoPrint, checks printer state, starts jobs, monitors completion, and proxies the printer webcam.

**Evidence:**

- Printer service application: https://github.com/RePro3D-Praxisproject/repro3d-backend/blob/e8691fd3c425532656dde317806dc13d06abebde/PrinterService/src/main/java/org/repro3d/EntryPointPrinterService.java
- `@Service` controlling printers: https://github.com/RePro3D-Praxisproject/repro3d-backend/blob/e8691fd3c425532656dde317806dc13d06abebde/PrinterService/src/main/java/org/repro3d/service/PrinterService.java

**Signature crime:** a Spring service sends the command that begins depositing hot plastic into the physical world.

---

## 🫘🫘🫘🫘 Rocket-Show/rocketshow — Spring Boot runs the stage show

**Category:** MIDI / DMX / audio / video / Raspberry Pi

**Repository:** https://github.com/Rocket-Show/rocketshow

**Why it is here:** Rocket Show is a Spring Boot application for automating audio, video, MIDI, and DMX lighting on Raspberry Pi systems.

**Evidence:**

- Spring Boot application: https://github.com/Rocket-Show/rocketshow/blob/038500ee0b82d95ab4200c1d6bc107a26127e2eb/src/main/java/com/ascargon/rocketshow/RocketShowApplication.java
- MIDI input service: https://github.com/Rocket-Show/rocketshow/blob/038500ee0b82d95ab4200c1d6bc107a26127e2eb/src/main/java/com/ascargon/rocketshow/midi/MidiDeviceInService.java
- MIDI output service: https://github.com/Rocket-Show/rocketshow/blob/038500ee0b82d95ab4200c1d6bc107a26127e2eb/src/main/java/com/ascargon/rocketshow/midi/MidiDeviceOutService.java
- MIDI-to-lighting conversion: https://github.com/Rocket-Show/rocketshow/blob/038500ee0b82d95ab4200c1d6bc107a26127e2eb/src/main/java/com/ascargon/rocketshow/lighting/DefaultMidi2LightingConvertService.java

**Signature crime:** the house lights await application-context refresh.

---

## 🫘🫘🫘🫘 rucko24/EspFlow — microcontroller flashing as a Spring service

**Category:** firmware / ESP32 / ESP8266

**Repository:** https://github.com/rucko24/EspFlow

**Why it is here:** the application is Spring Boot and its `EsptoolService` is a Spring `@Service` that discovers ESP devices and performs flash-related operations through `esptool`.

**Evidence:**

- Spring Boot application: https://github.com/rucko24/EspFlow/blob/ad82acad3a49cbea46c396b6e06f0b5acf608a9f/src/main/java/com/esp/espflow/Application.java
- Flash tooling service: https://github.com/rucko24/EspFlow/blob/ad82acad3a49cbea46c396b6e06f0b5acf608a9f/src/main/java/com/esp/espflow/service/EsptoolService.java

**Signature crime:** firmware flashing has dependency injection.

---

## 🫘🫘🫘 NextGPUNetwork/nextgpu-app — microphone capture inside `@Service`

**Category:** live audio I/O

**Repository:** https://github.com/NextGPUNetwork/nextgpu-app

**Why it is here:** the Spring service owns a `TargetDataLine`, opens the microphone, starts a dedicated recording thread, reads PCM buffers, and calculates amplitude.

**Evidence:**

- Spring Boot application: https://github.com/NextGPUNetwork/nextgpu-app/blob/4d693280d7d8ca89b18632f7e261213dd47c19a0/agent/src/main/kotlin/ai/nextgpu/agent/NextGpuAgentApplication.kt
- Audio recorder service: https://github.com/NextGPUNetwork/nextgpu-app/blob/4d693280d7d8ca89b18632f7e261213dd47c19a0/agent/src/main/kotlin/ai/nextgpu/agent/service/AudioRecorderService.kt

**Signature crime:** the microphone is one annotation away from enterprise governance.

---

## 🫘🫘🫘 ANnianExplorer/aiToVoice — pitch DSP in a Spring service

**Category:** audio DSP

**Repository:** https://github.com/ANnianExplorer/aiToVoice

**Why it is here:** a Spring `@Service` constructs a TarsosDSP `AudioDispatcher` and YIN `PitchProcessor`, processes audio, and calculates pitch statistics.

**Evidence:**

- Spring Boot application: https://github.com/ANnianExplorer/aiToVoice/blob/cc4213d75afd0c300d8bd951d9020bce8bb3ca19/backend/src/main/java/com/aitovoice/AitoVoiceApplication.java
- Pitch analyzer: https://github.com/ANnianExplorer/aiToVoice/blob/cc4213d75afd0c300d8bd951d9020bce8bb3ca19/backend/src/main/java/com/aitovoice/voice/service/PitchAnalyzer.java

**Signature crime:** `PitchAnalyzer` needed to be a managed service, apparently.

---

## 🫘🫘🫘 tckb/busylight_rest — a USB lamp gets a REST API

**Category:** USB hardware

**Repository:** https://github.com/tckb/busylight_rest

**Why it is here:** a Spring application wraps the driver for a physical Busylight device and exposes operations for status, color, and tone.

**Evidence:**

- Spring configuration and driver bean: https://github.com/tckb/busylight_rest/blob/a0252874a5b2e4915d897076786fe7897e45b40d/src/main/java/com/fyayc/essen/busylight/server/ServerConfig.java
- Physical-device driver: https://github.com/tckb/busylight_rest/blob/a0252874a5b2e4915d897076786fe7897e45b40d/src/main/java/com/fyayc/essen/busylight/server/driver/BusylightDriver.java

**Signature crime:** HTTP makes a USB desk lamp change color through Spring.

---

# Honorable mentions / queue for deeper writeups

These deserve dedicated entries once their strongest source evidence is collected:

- `f8573/JLC` — Spring Boot application containing JCuda/JCublas matrix multiplication paths.
- JavaGPT — local LLM inference served with Spring Boot.
- Deep Java Library Spring Boot Starter — inference auto-configuration.
- Spring AI local ONNX transformer embeddings.
- Spring Boot Starter Spigot — Spring inside Minecraft plugins.
- Raspberry Pi Spring Boot alarm systems and GPIO projects.
- OpenWMS communication driver — automation / PLC / Raspberry Pi integrations.
- `iot-dc3` industrial drivers beyond CAN: Modbus TCP, Siemens S7, OPC UA, OPC DA, MQTT, BLE, KNX, and friends.
- packet capture / DPI applications built around Pcap4J `@Service` classes.

If you can prove a worse boundary, open a contribution.
