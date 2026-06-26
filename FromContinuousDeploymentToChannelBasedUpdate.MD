[https://en.wikipedia.org/wiki/Continuous_deployment](https://en.wikipedia.org/wiki/Continuous_deployment)
>Continuous deployment (CD) is a software engineering approach in which software functionalities are delivered frequently and through automated deployments. Continuous deployment contrasts with continuous delivery (also abbreviated CD), a similar approach in which software functionalities are also frequently delivered and deemed to be potentially capable of being deployed, but are actually not deployed.[4] As such, continuous deployment can be viewed as a more complete form of automation than continuous delivery

Multiple channel
- release
- preview
- daily

Available solution:

### 1. dotnet tool deployment
- No channel support, only latest available
- Bandwidth: No issue, hosted by nuget.org
- Use case: CLI tools and simple applications

### 2. Squirrel.Windows
**Link:** https://github.com/Squirrel/Squirrel.Windows/

**Status:** Actively maintained, seeking community contributions for ongoing support.

**Features:**
- Delta updates (efficient bandwidth)
- Multiple channels (stable, beta, nightly)
- Auto-updates with user control
- Works with NSIS or custom installers

**Considerations:**
- Requires hosting your own update server or use third-party service
- Can integrate with NuGet feeds for hosting
- Suitable for desktop applications needing frequent updates across multiple release channels

### 3. Windows Package Manager (winget)
**Repository:** https://github.com/microsoft/winget-pkgs

**Overview:**
Distribution channel through Windows Package Manager (native in Windows 11 and available for Windows 10).

**How it works:**
- Submit package manifest to microsoft/winget-pkgs repository
- Manifest specifies package metadata, download URL, and SHA256 hash
- Users discover and install via `winget install package-name`
- Users can update via `winget upgrade`

**Manifest includes:**
- Package ID, version, publisher
- Download URL and SHA256 hash
- Installation settings (silent install flags)
- License, homepage, documentation links

**Advantages:**
- Native Windows integration (built-in to Windows 11)
- Discoverability through winget search
- Automatic update notifications for users
- Free distribution channel

**Considerations:**
- Submission reviewed by Microsoft (typically 1-2 weeks)
- Each version requires separate manifest submission
- Best for stable releases, not continuous deployment
- Can be combined with Squirrel.Windows for delta updates between releases

**Best practice:**
Use Squirrel.Windows for frequent automatic updates (preview, daily channels), and publish stable release versions to winget for users who prefer manual updates or easy installation.
