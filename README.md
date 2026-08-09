name: Build APK

on:
  workflow_dispatch:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Extract Android project
        run: |
          mkdir android-project
          unzip -q "MahmoudStudentPlanner_AndroidProject (44).zip" -d android-project

      - name: Set up Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: "17"

      - name: Set up Android SDK
        uses: android-actions/setup-android@v3

      - name: Install Android SDK
        run: sdkmanager "platform-tools" "platforms;android-35" "build-tools;35.0.0"

      - name: Set up Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: "8.7"

      - name: Build APK
        working-directory: android-project
        run: gradle assembleDebug --no-daemon

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: MahmoudStudentPlanner-APK
          path: android-project/app/build/outputs/apk/debug/app-debug.apk
