Studio Builder on the Workday Developer Site is an assembly editor but not a full Java IDE. To work on any Java code in your integration, you can export it from Studio Builder as a Gradle project, edit and build it in your local IDE of choice, then upload the resulting output back into the same collection.

The export is a standard Gradle project, so it will be compatible with the IntelliJ IDE, VS Code, Eclipse, or any other Gradle-aware IDE without further setup.

1. From your Studio Collection in Studio Builder, open the three dots menu (next to **Validate**) and click **Download Archive**. Studio Builder initiates the download of `<collectionName>.zip` — a self-contained Gradle project with one subfolder for each integration in the collection, plus a `workday-api/` folder of compile-only Integration Runtime v2 (IR2) API JARs.
2. Unzip and open the project in your IDE. Edit Java in `<IntegrationName>/src/main/java/`. Place any third-party JARs your integration depends on in `<IntegrationName>/lib/`.
3. macOS and *nix users: Run `chmod +x gradlew`, then run `./gradlew build` from the project root.Windows users: Run `gradlew.bat`.Gradle only rebuilds the integrations whose Java has actually changed. Unchanged subprojects are skipped. The build produces a single upload archive at `build/source.zip`.
4. Back in Studio Builder, open the same **Related Actions** menu and click **Upload Archive**.
5. Select `source.zip` from the `build/` folder. The maximum size allowed for the zip file is 100 MB.
6. Click **Review** to see a preview of every file that will be added, modified, or removed.
7. Click **Upload** to apply the changes.
8. Deploy the collection as you would for any change.

### Directory Restrictions and Constraints

Inside each integration subfolder, only `src/` and `ws/` are accepted by the upload. Anything else is rejected. The Gradle build that ships in the export already produces a clean archive, so reuploading `build/source.zip` is the safe path.

The upload can update Java code in integrations that already exist in your collection, but it cannot create new integration projects. Create those in Studio Builder first.

### Errors and Responses

This table summarizes errors and responses you might encounter:

|**Message**|**What it means**|**What to do**|
| --- | --- | --- |
|Only .zip build archive files are allowed.|The file you selected is not a .zip.|Upload the `build/source.zip` from your Gradle build.|
|File size must be less than 100 MB.|Your archive is larger than the upload limit allows.|Rebuild, following step 3 above, and upload the resulting `source.zip` rather than zipping the source folder by hand.|
|Source archive contains invalid folder(s). Only `src/` and `ws/` folders are allowed inside integration projects.|An integration folder in the archive contains something other than `src/` or `ws/`.|Remove the extra folders or use `build/source.zip`, which excludes them automatically.|
|Source import cannot create new projects. Unknown project(s) in archive: `<name>`.|The archive contains a top-level folder that isn't an integration in the collection.|Remove the unknown folder. To add a new integration, create it in Studio Builder first, then re-export.|
|Build archive uploaded successfully.|Studio Builder finished applying your changes.|Continue working in Studio Builder, or deploy.|