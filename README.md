# Minimal reproducible example

## Get started

1. Create fresh empty app

   ```bash
   npx create-expo-app@latest
   ```

2. Add `expo-image-picker` and `expo-localization`

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
4. Add test text and links in `app/(tabs)/index.ts`

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
   
