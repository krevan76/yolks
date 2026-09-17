# Yolks

A curated collection of core images that can be used with Pterodactyl's Egg system. Each image is rebuilt
periodically to ensure dependencies are always up-to-date.

This is the **LunaryMC fork** (`krevan76/yolks`). Images are published to **GHCR under the `krevan76`
namespace**, so every image/tag works out of the box with no extra configuration.

## How images are named

Everything is grouped into a few GHCR packages, split by **tags** (one tag per image + version):

| Package | Tag format | Example |
|---|---|---|
| `ghcr.io/krevan76/yolks` | `<image>_<version>` | `ghcr.io/krevan76/yolks:java_21` |
| `ghcr.io/krevan76/yolks` | `<os>` (base images) | `ghcr.io/krevan76/yolks:debian` |
| `ghcr.io/krevan76/games` | `<game>` | `ghcr.io/krevan76/games:source` |
| `ghcr.io/krevan76/installers` | `<os>` | `ghcr.io/krevan76/installers:alpine` |
| `ghcr.io/krevan76/apps` | `<app>` | `ghcr.io/krevan76/apps:uptimekuma` |

In a Pterodactyl Egg you only set the image inside the Egg's `Docker Images` field, e.g.:

```
ghcr.io/krevan76/yolks:java_21
ghcr.io/krevan76/yolks:nodejs_22
ghcr.io/krevan76/yolks:graalvm_21
```

All images are available for `linux/amd64` and `linux/arm64` unless otherwise noted — on an ARM host no
tag change is needed, the multi-arch image just works.

## Catalog

### Languages & runtimes

| Image | Tags | Base |
|---|---|---|
| `java_*` | `8` `11` `16` `17` `19` `21` `22` `25` | Eclipse Temurin (JDK) |
| `graalvm_*` | `17` `20` `21` `22` `23` | GraalVM Community (Native Image) |
| `nodejs_*` | `12` `14` `16` `17` `18` `19` `20` `21` `22` `23` `24` | Node.js |
| `deno_*` | `1` `2` | Deno |
| `bun_*` | `latest` `canary` | Bun |
| `python_*` | `2.7` `3.7` `3.8` `3.9` `3.10` `3.11` `3.12` `3.13` | Python |
| `php_*` | `8.1` `8.2` `8.3` `8.4` | PHP (CLI, `pdo_sqlite`) |
| `ruby_*` | `3.2` `3.3` `3.4` | Ruby |
| `go_*` | `1.14` `1.15` `1.16` `1.17` `1.18` `1.19` `1.20` `1.21` `1.22` `1.23` | Go |
| `rust_*` | `1.56` `1.60` `latest` | Rust |
| `dotnet_*` | `2.1` `3.1` `5` `6` `7` `8` `9` | .NET |
| `dart_*` | `2.17` `2.18` `2.19` `3.3` `stable` | Dart |
| `elixir_*` | `1.12` `1.13` `1.14` `1.15` `latest` | Elixir |
| `erlang_*` | `22` `23` `24` `25` `26` | Erlang/OTP |
| `mono_*` | `latest` | Mono |

### Databases

| Image | Tags |
|---|---|
| `mongodb_*` | `5` `6` `7` `8` |
| `redis_*` | `6` `7` `8` |

### Servers & apps

| Image / Tag | Purpose |
|---|---|
| `steamcmd_*` → `debian` `ubuntu` `dotnet` `proton` `proton_8` `sniper` | SteamCMD / Proton |
| `voice_*` → `teaspeak` `mumble` | Voice servers |
| `ghcr.io/krevan76/apps:uptimekuma` | Uptime Kuma |
| `ghcr.io/krevan76/games:source` | Source-engine games |

### Base OS images

| Tag | Base |
|---|---|
| `alpine` `debian` `ubuntu` | Minimal OS images with the `container` user + entrypoint |
| `box64` | box64 (x86_64 emulation on ARM) |

### Installers

Install-script helper images (only for Egg install scripts, not for running):

`ghcr.io/krevan76/installers:` `alpine` `debian` `ubuntu`

## Using the images

Pull any image directly:

```bash
docker pull ghcr.io/krevan76/yolks:nodejs_22
docker pull ghcr.io/krevan76/yolks:graalvm_21
```

Every image runs as the non-root `container` user with `/home/container` as workdir and
`/entrypoint.sh` (via `tini`) as entrypoint, so it drops straight into a Pterodactyl/Pelican Egg.

## Contributing

When adding a new version to an existing image, such as `java v42`, add it within a child folder of `java`
(`java/42/Dockerfile` for example) and update the matching `.github/workflows/*.yml` matrix so it gets
tagged and built. New image families need their own folder, `entrypoint.sh` and workflow — use `graalvm`,
`deno`, `php` or `ruby` as a reference.
