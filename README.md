# Minimal reproducible example

## Get started

1. Create fresh empty app

   ```bash
   npx create-expo-app@latest
   ```

2. Add `expo-localization` and `expo-image-picker`(for demonstating purposes why this error should not be ignored)

   ```bash
   yarn add expo-image-picker expo-localization
   ```
3. In the `app.json` add localization plugin to enable per-app language selection via system settings

   ```json
   "plugins": [
      //...
      [
        "expo-localization",
        {
          "supportedLocales": {
            "ios": ["en", "de"],
            "android": ["en", "de"]
          }
        }
      ]
    ],
   ```
4. Add test text and links in `app/(tabs)/index.ts` for easy testing

   ```js
   <ThemedView style={styles.stepContainer}>
        <ThemedText type="subtitle">Test image picker</ThemedText>
        <ThemedText type="link" onPress={pickImageAsync}>
          Pick an image
        </ThemedText>
      </ThemedView>
      <ThemedView style={styles.stepContainer}>
        <ThemedText type="subtitle">User languages</ThemedText>
        <ThemedText>
          {deviceLanguages.map((locale) => locale.languageCode).join(", ")}
        </ThemedText>
        <ThemedText type="link" onPress={() => Linking.openSettings()}>
          Change language
        </ThemedText>
      </ThemedView>
   ```
5. Build and run

   ```bash
   npx expo run:android
   ```
   
# Steps to reproduce

1. Run the app
2. Click on `Pick an image` -> `ImagePicker` will open as it should
3. Click on `Change language` and pick another language
4. Return to the app and there is an error:

   ```
   Looks like you have configured linking in multiple places.
   This is likely an error since deep links should only be handled in one place to avoid conflicts.
   Make sure that:
   - You don't have multiple NavigationContainers in the app each with 'linking' enabled
   - Only a single instance of the root component is rendered
   - You have set 'android:launchMode=singleTask' in the '<activity />' section of the 'AndroidManifest.xml' file to avoid launching multiple instances Error Stack
   ```
5. Ignore this error and click on `Pick an image` -> `ImagePicker` doesn't open and there is an error

   ```
   Uncaught (in promise, id: 0) Error: Call to function 'ExponentImagePicker.launchImageLibraryAsync' has been rejected.
   → Caused by: java.lang.IllegalStateException: Attempting to launch an unregistered ActivityResultLauncher
   with contract expo.modules.imagepicker.contracts.ImageLibraryContract@670bf83 and
   input ImageLibraryContractOptions(options=expo.modules.imagepicker.ImagePickerOptions@aada500).
   You must ensure the ActivityResultLauncher is registered before calling launch()
   ```
# Notes

This issue is only on Android and app needs full restart to function properly again until the next language change.
