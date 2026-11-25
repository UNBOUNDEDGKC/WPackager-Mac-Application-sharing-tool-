# WPackager Mac Application sharing tool
For people who can't figure out how to export their Swift apps. You're welcome! (having to figure out how to set it up yourself is a nightmare)

Also do everything written below (looks long but it's pretty easy compared to finding it out yourself)

# Xcode Project Settings for Maximum Compatibility

## 1. Signing & Capabilities
In Xcode, go to **Project > Targets > [Your App] > Signing & Capabilities**:

* **Remove "App Sandbox" (Recommended for tools & utilities):** Click the trash icon next to *App Sandbox* if your app needs to read/write user files outside its container or execute terminal commands. Only keep it enabled if your app is fully self-contained.
* **Turn ON "Automatically manage signing" (or select "Sign to Run Locally"):** Select your free *Personal Team* or set *Signing Certificate* to **Sign to Run Locally** so Xcode does not require a paid distribution certificate.
* **Remove "Hardened Runtime" (if present):** Delete *Hardened Runtime* if you are not notarizing the app with Apple. This prevents macOS from blocking certain embedded libraries or scripts.

## 2. Build Settings
Go to **Project > Targets > [Your App] > Build Settings** (make sure **All** is selected instead of *Basic*):

* **Architectures (`ARCHS`):** Set this to **Standard Architectures (Apple Silicon, Intel)** (`arm64 x86_64`). This builds a *Universal App* that runs natively on both modern M1/M2/M3/M4 Macs and older Intel Macs.
* **Build Active Architecture Only (`ONLY_ACTIVE_ARCH`):** Set **Release** to **No**. If set to *Yes*, your exported app will only run on the exact processor architecture of the Mac used to build it.
* **macOS Deployment Target (`MACOSX_DEPLOYMENT_TARGET`):** Set this as low as your codebase allows (such as **macOS 11.0** or **macOS 12.0**). A lower target ensures users on older macOS versions can still launch your app.

## 3. Exporting from Xcode
* In the top menu bar, select **Product > Archive**.
* In the Organizer window that appears, click **Distribute App** > **Custom** > **Copy App** (do not choose *App Store Connect*).
* Run the exported `.app` bundle through **WPackager** to re-sign and compress it into a distributable `.zip` file.

## 4. Instructions for End Users
Because the app is ad-hoc signed without a paid Apple Developer ID, recipients will see a one-time macOS Gatekeeper warning on first launch. Include these 3 steps when sharing your `.zip`:

1. Unzip the archive and try opening the app once (click **Done** or **Cancel** on the security prompt).
2. Open **System Settings > Privacy & Security**.
3. Scroll down to the **Security** section and click **Open Anyway** next to the app's name.
