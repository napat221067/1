# name: Build IPA

on:
  push:
    branches: [ main ]

jobs:
  build-ipa:
    runs-on: macos-latest
    env:
      APP_ID: com.aimbot.simulator
      SCHEME: aimbot
      EXPORT_METHOD: development

    steps:
      - uses: actions/checkout@v3

      - name: Install Fastlane
        run: sudo gem install fastlane -NV

      - name: Build & Export IPA
        run: |
          fastlane build \
            scheme:"${{ env.SCHEME }}" \
            export_method:"${{ env.EXPORT_METHOD }}" \
            output_directory:"./output" \
            output_name:"aimbot.ipa"

      - name: Upload IPA Artifact
        uses: actions/upload-artifact@v3
        with:
          name: aimbot-ipa
          path: output/aimbot.ipa
