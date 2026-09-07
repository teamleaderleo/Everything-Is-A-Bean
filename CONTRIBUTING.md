# Contributing

Thank you for helping preserve evidence that every subsystem is only one annotation away from becoming enterprise Java.

## Submission standard

A good entry needs three things:

1. **A real public project.** No hypothetical snippets, parody repos, or generated examples unless they are clearly labeled as such.
2. **A direct Spring connection.** Show the Spring annotation, bean definition, application context, Boot entry point, or configuration that reaches the unusual subsystem.
3. **A direct subsystem connection.** Show the motor controller, renderer, filesystem operation, CAN frame, DSP loop, CUDA path, serial command, VM operation, firmware write, etc.

The strongest submissions let a reviewer trace a short path like:

```text
Spring-managed object
    -> low-level API
    -> physical / OS / compute effect
```

## What to include

Please include:

- project name and repository URL;
- category;
- one-sentence summary of the crime;
- direct source link proving Spring involvement;
- direct source link proving low-level / unusual behavior;
- proposed curse score from 1 to 5 beans;
- optional commentary explaining why the boundary is especially funny.

## Curse score guide

### 🫘 — ordinary

Normal web/backend use. Usually not catalog-worthy by itself.

### 🫘🫘 — unusual habitat

Spring in a desktop app, CLI, game, embedded-ish tool, or other nonstandard environment.

### 🫘🫘🫘 — direct device / compute involvement

Spring-managed code talks directly to serial devices, audio hardware, packet capture, GPU compute, cameras, USB devices, etc.

### 🫘🫘🫘🫘 — owns a low-level subsystem

Examples: renderer, storage engine, VM lifecycle manager, firmware flasher, printer controller, industrial protocol driver.

### 🫘🫘🫘🫘🫘 — moves matter or impersonates the OS

Examples: robot motors as beans, G-code machine motion, telescope motion, CAN drivers, FUSE filesystems, real-time-ish signal paths.

## Bonus points

The committee is easily bribed by any of the following:

- physical actuator represented as a bean;
- `@Autowired` inside a hot path;
- Spring XML defining hardware;
- class names ending in `DriverApplication`;
- `@PreDestroy` shutting down a device;
- JNI used to move Spring closer to the metal;
- `application.yml` changing something measured in volts, hertz, baud, RPM, degrees, or samples/second;
- a stack trace that could plausibly prevent a wheel from turning.

## Please do not submit

- ordinary CRUD services;
- generic "Spring + database" applications;
- projects where Spring only serves a dashboard and never reaches the interesting subsystem;
- claims based only on a README when the source does not support them;
- harassment or ridicule directed at individual developers.

We are cataloguing technical juxtaposition, not putting maintainers on trial.

## Entry template

```md
## 🫘🫘🫘🫘 project/name — short title

**Category:**

**Repository:** https://github.com/...

**Why it is here:**

**Evidence:**

- Spring side: https://github.com/...
- cursed side: https://github.com/...

**Signature crime:** one sentence.
```

## Machine-readable catalog

If you add an entry to `CATALOG.md`, also add it to `projects.yml`.

Keep evidence URLs as direct source links whenever possible. Commit-pinned URLs are preferred for the strongest evidence because branches move.
