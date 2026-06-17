---
name: auto-shaconfigs-embed
description: Use this skill to configure a Shesha application project's .csproj file to automatically embed all .shaconfig files as Embedded Resources. Trigger when a user asks to set up auto-embedding of .shaconfig files, wants to avoid manually setting Build Action for each config migration file, or encounters deployment issues caused by missing embedded configuration exports.
---

# Auto-Embed .shaconfig Files

Configure the Application project's `.csproj` to automatically embed all `.shaconfig` files in `ConfigMigrations/` as Embedded Resources — no manual per-file setup needed.

## Step 1: Locate the Application .csproj

Find the `.csproj` file for the Application project (not Domain). It is typically named `<AppName>.Application.csproj` or similar.

## Step 2: Check for Existing Configuration

Search the `.csproj` for any existing `.shaconfig` entries:

```xml
<EmbeddedResource Include="ConfigMigrations\*.shaconfig"
```

- If individual `<EmbeddedResource>` entries exist for `.shaconfig` files, remove them — the wildcard pattern added in Step 3 will replace them.
- If the wildcard pattern already exists, the project is already configured. Inform the user and stop.

## Step 3: Add the Wildcard ItemGroups

Add the following two `ItemGroup` blocks inside the root `<Project>` element, preferably near the top with other `ItemGroup` entries:

```xml
<ItemGroup>
  <None Remove="ConfigMigrations\*.shaconfig" />
</ItemGroup>
<ItemGroup>
  <EmbeddedResource Include="ConfigMigrations\*.shaconfig" />
</ItemGroup>
```

### Why Two ItemGroups?

- `None Remove` — removes `.shaconfig` files from .NET's default file list to prevent a duplicate item build error.
- `EmbeddedResource Include` — embeds all matching files into the DLL.

Both entries are required. Omitting the first causes a build error.

## Step 4: Verify

Confirm the `.csproj` now contains both entries and no leftover individual `.shaconfig` `<EmbeddedResource>` lines remain.

### Important Notes

- The `ConfigMigrations/` folder must already exist in the project, or be created before `.shaconfig` files are dropped in.
- After this setup, any new `.shaconfig` file added to `ConfigMigrations/` is automatically embedded on the next build — no further `.csproj` edits required.
- This is a one-time setup per Application project.

## Example: Before and After

**Before** (manual, error-prone):
```xml
<ItemGroup>
  <EmbeddedResource Include="ConfigMigrations\package20240614_0804.shaconfig" />
</ItemGroup>
```

**After** (automatic):
```xml
<ItemGroup>
  <None Remove="ConfigMigrations\*.shaconfig" />
</ItemGroup>
<ItemGroup>
  <EmbeddedResource Include="ConfigMigrations\*.shaconfig" />
</ItemGroup>
```
