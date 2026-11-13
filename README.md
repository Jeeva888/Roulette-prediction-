name: Build Roulette 50 APK

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Setup Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.9'

      - name: Setup Android SDK
        uses: android-actions/setup-android@v3

      - name: Extract project
        run: |
          unzip source.zip -d extracted
          cd extracted/Roulette50-Android
          echo "PROJECT_DIR=$PWD" >> $GITHUB_ENV

      - name: Install Android SDK
        run: sdkmanager "platforms;android-35" "build-tools;35.0.0"

      - name: Build APK
        run: |
          cd "$PROJECT_DIR"
          gradle assembleDebug

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: Roulette50-APK
          path: extracted/Roulette50-Android/app/build/outputs/apk/debug/app-debug.apk
  
