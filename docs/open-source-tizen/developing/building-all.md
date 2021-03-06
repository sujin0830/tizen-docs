# Building All Packages Locally with GBS

This topic provides information on how to perform a build for all Tizen packages using Git Build System (GBS).

You must read, understand, and correctly follow the instructions in the following documents before performing the build:

- [Setting up Development Environment](setting-up.md)
- [Installing Development Tools](installing.md)
- [Building Packages Locally with GBS](building.md)

> **Note**
>
> - To get a transparent understanding of the local full build commands, see [GBS Build Concepts](#gbs-build-concepts).
> - To speed up the build process when hardware allows, see [Speeding up a Local Build](building.md#speed).

## Cloning All Tizen Projects

You can clone all Tizen projects over SSH or HTTPS.

> **Note**
> Procedures to clone all projects over SSH and HTTPS are almost identical. The only difference is the git cloning protocol. However, you can only contribute code to Tizen using the SSH method.

### Cloning All Projects over SSH <a name="#over-ssh"></a>

You can clone the source of all projects over SSH, including the latest source and the snapshot source.

> **Note**
> Make sure the SSH configuration file is correctly configured according to [Setting up Development Environment](setting-up.md). Otherwise, the synchronization cannot be performed successfully and the following error message is displayed:

```
...
nc: connection failed, SOCKS error 1
ssh_exchange_identification: Connection closed by remote host
nc: connection failed, SOCKS error 1
...
fatal: Could not read from remote repository.

Please make sure you have the correct access rightsand the repository exists.
ssh_exchange_identification: Connection closed by remote host
fatal: Could not read from remote repository.
```

To clone the latest sources of all projects over SSH:

> **Note**
> Latest source does not mean the latest sources in the Gerrit server, but the reference snapshot sources where images generated by the reference snapshot are guaranteed by passing all test cases.

1. Create a new directory for Tizen and switch to it:

   ```bash
   $ mkdir ~/<Tizen_Project> && cd ~/<Tizen_Project>
   ```

2. Create a `.repo/` directory that contains all the information - for example, git repositories, commit IDs, and remote URLs - to download all git repositories to construct all Tizen projects.
   Initialize the repository by executing the following command:

   ```bash
   $ repo init -u ssh://<Username>@review.tizen.org:29418/scm/manifest -b <tizen branch> -m ${profile}_${repository}.xml
   ```

   `<tizen branch>` is `tizen` for Tizen 4.0 and `tizen_3.0` for Tizen 3.0.

   For example:

   - Tizen 4.0 Unified / standard

     ```bash
     $ repo init -u ssh://<Username>@review.tizen.org:29418/scm/manifest -b tizen -m unified_standard.xml
     ```

   - Tizen 4.0 Unified / emulator

     ```bash
     $ repo init -u ssh://<Username>@review.tizen.org:29418/scm/manifest -b tizen -m unified_emulator.xml
     ```

   - Tizen 3.0 Common / arm-wayland

     ```bash
     $ repo init -u ssh://<Username>@review.tizen.org:29418/scm/manifest -b tizen_3.0 -m common_arm-wayland.xml
     ```

   - Tizen 3.0 Common / arm64-wayland

     ```bash
     $ repo init -u ssh://<Username>@review.tizen.org:29418/scm/manifest -b tizen_3.0 -m common_arm64-wayland.xml
     ```

   - Tizen 3.0 Common / emulator32-wayland

     ```bash
     $ repo init -u ssh://<Username>@review.tizen.org:29418/scm/manifest -b tizen_3.0 -m common_emulator32-wayland.xml
     ```

   - Tizen 3.0 Common / ia32-wayland

     ```bash
     $ repo init -u ssh://<Username>@review.tizen.org:29418/scm/manifest -b tizen_3.0 -m common_ia32-wayland.xml
     ```

   - Tizen 3.0 Common / x86_64-wayland

     ```bash
     $ repo init -u ssh://<Username>@review.tizen.org:29418/scm/manifest -b tizen_3.0 -m common_x86_64-wayland.xml
     ```

   - Tizen 3.0 Mobile / arm-wayland

     ```bash
     $ repo init -u ssh://<Username>@review.tizen.org:29418/scm/manifest -b tizen_3.0 -m mobile_arm-wayland.xml
     ```

   - Tizen 3.0 Mobile / emulator-wayland

     ```bash
     $ repo init -u ssh://<Username>@review.tizen.org:29418/scm/manifest -b tizen_3.0 -m mobile_emulator-wayland.xml
     ```

   - Tizen 3.0 Mobile / target-TM1

     ```bash
     $ repo init -u ssh://<Username>@review.tizen.org:29418/scm/manifest -b tizen_3.0 -m mobile_target-TM1.xml
     ```

   - Tizen 3.0 TV / arm-wayland

     ```bash
     $ repo init -u ssh://<Username>@review.tizen.org:29418/scm/manifest -b tizen_3.0 -m tv_arm-wayland.xml
     ```

   - Tizen 3.0 TV / emulator32-wayland

     ```bash
     $ repo init -u ssh://<Username>@review.tizen.org:29418/scm/manifest -b tizen_3.0 -m tv_emulator32-wayland.xml
     ```

   - Tizen 3.0 TV / emulator64-wayland

     ```bash
     $ repo init -u ssh://<Username>@review.tizen.org:29418/scm/manifest -b tizen_3.0 -m tv_emulator64-wayland.xml
     ```

   - Tizen 3.0 Wearable / emulator-circle

     ```bash
     $ repo init -u ssh://<Username>@review.tizen.org:29418/scm/manifest -b tizen_3.0 -m wearable_emulator-circle.xml
     ```

   - Tizen 3.0 Wearable / target-circle

     ```bash
     $ repo init -u ssh://<Username>@review.tizen.org:29418/scm/manifest -b tizen_3.0 -m wearable_target-circle.xml
     ```

   - Tizen 3.0 Wearable / emulator32-wayland

     ```bash
     $ repo init -u ssh://<Username>@review.tizen.org:29418/scm/manifest -b tizen_3.0 -m wearable_emulator32-wayland.xml
     ```

   - Tizen 3.0 IVI /arm

     ```bash
     $ repo init -u ssh://<Username>@review.tizen.org:29418/scm/manifest -b tizen_3.0 -m ivi_arm.xml
     ```

   - Tizen 3.0 IVI / emulator

     ```bash
     $ repo init -u ssh://<Username>@review.tizen.org:29418/scm/manifest -b tizen_3.0 -m ivi_emulator.xml
     ```

3. Clone the specific snapshot source of all projects over SSH (optional). <a name="clone-ssh-snapshot"></a>  
   Replace the latest manifest with a snapshot manifest and make proper modifications by executing the following two commands, as appropriate:

   ```bash
   $ wget  <Snapshot_Manifest_URL> -O .repo/manifests/<profile>/<repository>/projects.xml
   $ sed -i '3,4d' .repo/manifests/<profile>/<repository>/projects.xml
   ```

   For example:

   - Tizen 4.0 Unified / standard

     ```bash
     $ wget http://download.tizen.org/snapshots/tizen/unified/tizen-unified_20170627.1/builddata/manifest/tizen-unified_20170627.1_standard.xml -O .repo/manifests/unified/standard/projects.xml
     $ sed -i '3,4d' .repo/manifests/unified/standard/projects.xml
     ```

   - Tizen 3.0 Common / arm64-wayland

     ```bash
     $ wget  http://download.tizen.org/snapshots/tizen/3.0-common/tizen-3.0-common_20170627.1/builddata/manifest/tizen-3.0-common_20170627.1_arm64-wayland.xml -O .repo/manifests/common/arm64-wayland/projects.xml
     $ sed -i '3,4d' .repo/manifests/common/arm64-wayland/projects.xml
     ```

   - Tizen 3.0 Mobile / target-TM1

     ```bash
     $ wget  http://download.tizen.org/snapshots/tizen/3.0-mobile/tizen-3.0-mobile_20170627.1/builddata/manifest/tizen-3.0-mobile_20170627.1_target-TM1.xml -O .repo/manifests/mobile/target-TM1/projects.xml
     $ sed -i '3,4d' .repo/manifests/mobile/target-TM1/projects.xml
     ```

   - Tizen 3.0 TV / arm-wayland

     ```bash
     $ wget  http://download.tizen.org/snapshots/tizen/3.0-tv/tizen-3.0-tv_20170627.1/builddata/manifest/tizen-3.0-tv_20170627.1_arm-wayland.xml -O .repo/manifests/tv/arm-wayland/projects.xml
     $ sed -i '3,4d' .repo/manifests/tv/arm-wayland/projects.xml
     ```

   - Tizen 3.0 Wearable / emulator-circle

     ```bash
     $ wget  http://download.tizen.org/snapshots/tizen/3.0-wearable/tizen-3.0-wearable_20170627.1/builddata/manifest/tizen-3.0-wearable_20170627.1_emulator-circle.xml -O .repo/manifests/wearable/emulator-circle/projects.xml
     $ sed -i '3,4d' .repo/manifests/wearable/emulator-circle/projects.xml
     ```

   - Tizen 3.0 IVI / arm

     ```bash
     $ wget  http://download.tizen.org/snapshots/tizen/3.0-ivi/tizen-3.0-ivi_20170627.1/builddata/manifest/tizen-3.0-ivi_20170627.1_arm.xml -O .repo/manifests/ivi/arm/projects.xml
     $ sed -i '3,4d' .repo/manifests/ivi/arm/projects.xml
     ```

4. Synchronize the files for all the projects based on the information downloaded by the `repo init` command by executing the following command:
   ```bash
   $ repo sync
   ```

### Cloning All Projects over HTTPS <a name="over-https"></a>

You can clone the source of all projects over HTTPS, including the latest source and the snapshot source.

To clone the latest sources of all projects over HTTPS:

1. Create a new directory for Tizen and switch to it:

   ```bash
   $ mkdir ~/<Tizen_Project> && cd ~/<Tizen_Project>
   ```

2. Create a `.repo/` directory.
   Initialize the repository by executing the following command:

   ```bash
   $ repo init -u https://git.tizen.org/cgit/scm/manifest -b <tizen branch> -m unified_${repository}.xml
   ```

   `<tizen branch>` is `tizen` for Tizen 4.0 and `tizen_3.0` for Tizen 3.0.

   > **Note** 
   > The `repo init` command using HTTPS is almost identical to that of using SSH. The only difference is the parameter of the `'-u'` option. Replace `'ssh://<Username>@review.tizen.org:29418/scm/manifest'` with `'https://git.tizen.org/cgit/scm/manifests'`.

   For example:

   - Tizen 4.0 Unified / standard

     ```bash
     $ repo init -u https://git.tizen.org/cgit/scm/manifest -b tizen -m unified_standard.xml
     ```

   - Tizen 4.0 Unified / emulator

     ```bash
     $ repo init -u https://git.tizen.org/cgit/scm/manifest -b tizen -m unified_emulator.xml
     ```

   - Tizen 3.0 Common / arm64-wayland

     ```bash
     $ repo init -u https://git.tizen.org/cgit/scm/manifest -b tizen_3.0 -m common_arm64-wayland.xml
     ```

   - Tizen 3.0 Mobile / target-TM1

     ```bash
     $ repo init -u https://git.tizen.org/cgit/scm/manifest -b tizen_3.0 -m mobile_target-TM1.xml
     ```

   - Tizen 3.0 TV / arm-wayland

     ```bash
     $ repo init -u https://git.tizen.org/cgit/scm/manifest -b tizen_3.0 -m tv_arm-wayland.xml
     ```

   - Tizen 3.0 Wearable / emulator-circle

     ```bash
     $ repo init -u https://git.tizen.org/cgit/scm/manifest -b tizen_3.0 -m wearable_emulator-circle.xml
     ```

   - Tizen 3.0 IVI /arm

     ```bash
     $ repo init -u https://git.tizen.org/cgit/scm/manifest -b tizen_3.0 -m ivi_arm.xml
     ```

3. Replace the remote 'fetch url' in the `_remote.xml` file by executing the following command:
   `$ sed -i 's/ssh:\/\/review.tizen.org/https:\/\/git.tizen.org\/cgit/' .repo/manifests/_remote.xml`The command changes the 'fetch url' in the `.repo/manifests/_remote.xml` file as follows:`from fetch="ssh://review.tizen.org/" to fetch="https://git.tizen.org/cgit/"`

4. Clone the specific snapshot source of all projects over HTTPS (optional).  
   Replace the latest manifest with a snapshot manifest and make proper modifications by executing the following two commands, as appropriate:

   ```bash
   $ wget  <Snapshot_Manifest_URL> -O .repo/manifests/<profile>/<repository>/projects.xml
   $ sed -i '3,4d' .repo/manifests/<profile>/<repository>/projects.xml
   ```

   > **Note**
   > This procedure is same as that of [SSH](#clone-ssh-snapshot).

5. Synchronize the files for all the projects based on the information downloaded by the `repo init` command by executing the following command:
   ```bash
   $ repo sync
   ```

## Building All Packages

Build all packages by executing the following commands:

```bash
$ cd <Tizen_Project>
$ gbs build <gbs build option>
```

For example:

```bash
$ cd <Tizen_Project>
$ gbs build -A i586 --threads=4 --clean-once
```

> **Note**
> Since the GBS configuration file (`.gbs.conf`) and build configuration file (`build.conf`) are also included in `scm/manifests`, they are automatically downloaded in the following paths after the `repo sync`:
>
> - `.gbs.conf`: `~/<Tizen_Project>/.gbs.conf`
> - `build.conf`: `~/<Tizen_Project>/scm/meta/build-config/<Tizen version>/<profile>/<repository>_build.conf`

## GBS Build Concepts <a name="gbs-build-concepts"></a>

The following build concepts help you transparently understand the full build commands:

- **Dependency cycles**
  - In Tizen versions higher than Tizen 2.4, packages with dependency cycles are only included in the Tizen:[<Tizen version>]:Base OBS project. And building packages with dependency cycles are not supported by GBS, so it is impossible to build all packages locally with GBS for Tizen:[<Tizen version>]:Base. Therefore, a remote repo which contains RPMs built by OBS for Tizen:[<Tizen version>]:Base needs to be added in the `.gbs.conf` file.
  - In Tizen versions lower than Tizen 2.3, there is only one OBS project and packages with dependency cycles are included in it. To continue the build by breaking the dependency cycles for Tizen versions lower than Tizen 2.3, see [Excluding Specific Packages](building.md#id3).
- **Accelerator packages**
  - Tizen provides cross-compilers and other accelerator packages. For higher than Tizen 3.0 versions, these accelerator packages are included in the Tizen:[<Tizen version>]:Base OBS project.
  - For Tizen versions lower than Tizen 2.3, there is only one OBS project and the accelerator packages are inlucded in it. These packages have names ending with `-x64` and need to be excluded to build locally with GBS. Otherwise, built out packages are installed and used, and it results in making accelerator packages fail to work. To exclude these accelerator packages for Tizen versions lower than Tizen 2.3, see [Excluding Specific Packages](building.md#id3).
