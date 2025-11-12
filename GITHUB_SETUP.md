# GitHub Setup Instructions

Quick guide to push this repository to your GitHub account.

## Step 1: Create New Repository on GitHub

1. Go to https://github.com/DreadedZombie
2. Click the **"+"** button (top right) → **New repository**
3. Fill in repository details:
   - **Name:** `LK5Pro-Marlin-Firmware` (or your preferred name)
   - **Description:** `Custom Marlin 2.0 firmware for Longer LK5 Pro with MKS Robin E3 v1.1 - Dual Blower + BLTouch support`
   - **Public** or **Private:** Your choice (Public recommended for community sharing)
   - **Do NOT** initialize with README (we already have one)
4. Click **Create repository**

## Step 2: Update Remote URL

Currently your local repo points to chaosoflife's GitHub. Change it to yours:

```bash
cd "c:\Users\ZOMBIEBOX\git\LK5Pro-Firmware\LK5-Dblower-BLtouch"

# Remove old remote
git remote remove origin

# Add your new remote (replace YOUR-REPO-NAME with actual name)
git remote add origin https://github.com/DreadedZombie/YOUR-REPO-NAME.git

# Verify it's correct
git remote -v
```

## Step 3: Review What Will Be Uploaded

Check what files have changed:
```bash
git status
```

**Current changes:**
- `Marlin/Configuration.h` - Modified (your custom config)
- `Marlin/Configuration_adv.h` - Modified (your advanced settings)
- `README.md` - Modified (updated with comprehensive docs)

**New files to decide on:**
- `DWIN_UI_CUSTOMIZATION.md` - Display customization guide (include or keep private?)
- `WIFI_MODULE_INSTALLATION.md` - WiFi setup guide (include or keep private?)

### Option A: Include All Documentation (Recommended)
This helps other LK5 Pro users with the same hardware!
```bash
# Add everything
git add .
```

### Option B: Keep Dev Notes Private
If you want to keep the WiFi and display guides private:
```bash
# Edit .gitignore and uncomment the last two lines
# Then add only specific files:
git add README.md
git add Marlin/Configuration.h
git add Marlin/Configuration_adv.h
git add "LK5 Pro BLtouch Pin Locations.png"
```

## Step 4: Commit Your Changes

```bash
# Create a commit with your changes
git commit -m "Add comprehensive documentation and custom configuration

- Updated README with feature list and credits
- Custom PID tuning for LK5 Pro
- Linear Advance, S-Curve, and Power Loss Recovery enabled
- Dual blower and BLTouch support configured
- Display firmware and WiFi module documentation added"
```

## Step 5: Push to Your GitHub

```bash
# Push to your repository
git push -u origin main
```

If prompted for credentials:
- Use your GitHub username
- Use a **Personal Access Token** as password (not your GitHub password)
  - Create token at: https://github.com/settings/tokens
  - Select scopes: `repo` (full control of private repositories)

## Step 6: Verify Upload

1. Go to https://github.com/DreadedZombie/YOUR-REPO-NAME
2. You should see:
   - Your updated README with all the documentation
   - All your custom configuration files
   - Credits section attributing original sources
   - GPL v3.0 license

## Step 7: Optional - Add Topics/Tags

On your GitHub repo page:
- Click the gear icon next to "About"
- Add topics: `marlin`, `3d-printer`, `longer-lk5-pro`, `mks-robin-e3`, `bltouch`, `firmware`
- This helps others find your repo!

## Step 8: Optional - Add a Release

When you're happy with your firmware:
1. Go to your repo → **Releases** → **Create a new release**
2. Tag: `v1.0.0`
3. Title: `Initial Release - LK5 Pro Custom Firmware`
4. Description:
```
Custom Marlin 2.0.x firmware for Longer LK5 Pro with MKS Robin E3 v1.1

Features:
- Linear Advance for improved print quality
- S-Curve Acceleration for smoother motion
- Power Loss Recovery
- Dual blower configuration
- BLTouch support (optional, easy to enable)
- Custom PID tuning
- Comprehensive documentation

Compatible Hardware:
- Longer LK5 Pro with MKS Robin E3 v1.1 board
- DWIN 4.3" touchscreen
- TMC2209 stepper drivers
```

5. Attach compiled firmware binary:
   - Upload `.pio/build/mks_robin_e3_maple/Robin_e3.bin`
   - Rename to `LK5Pro-Firmware-v1.0.0.bin` when uploading

---

## Future Updates

When you make changes:
```bash
# Make your changes, then:
git add .
git commit -m "Description of changes"
git push
```

## Troubleshooting

### "Permission denied" when pushing
- Make sure you're using a Personal Access Token, not your password
- Token must have `repo` scope enabled

### "Remote already exists"
```bash
# Remove and re-add with correct URL
git remote remove origin
git remote add origin https://github.com/DreadedZombie/YOUR-REPO-NAME.git
```

### Want to change what's uploaded?
```bash
# Reset staged changes
git reset

# Then selectively add files
git add FILE_NAME
```

---

**Done!** Your custom firmware is now on GitHub for the community to use and contribute to! 🚀
