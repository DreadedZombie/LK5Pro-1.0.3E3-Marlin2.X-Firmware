# DWIN Display UI Customization - Developer Notes

Internal documentation for customizing the DWIN 4.3" touchscreen UI.

## Display Specifications

- **Resolution:** 480x320 pixels
- **Format:** Binary image files (`.bin`)
- **Communication:** Serial UART (LCD_SERIAL_PORT 3, 115200 baud)
- **Processor:** Separate from main board (independent display controller)
- **Storage:** Internal flash memory for graphics/fonts

## File Structure

```
Firmware/assets/
├── bmp_*.bin          # UI graphics (buttons, icons, screens)
├── FontUNIGBK.bin     # Unicode font file
└── (150+ individual icon files)
```

### Key Files to Customize

| File | Size | Purpose |
|------|------|---------|
| `bmp_logo.bin` | 307,200 bytes | Boot splash screen (480x320) |
| `bmp_printing.bin` | 32,765 bytes | Print status screen |
| `bmp_pause.bin` | 24,005 bytes | Pause button |
| `bmp_resume.bin` | 24,005 bytes | Resume button |
| `bmp_stop.bin` | 24,005 bytes | Stop button |
| `bmp_back.bin` | 21,533 bytes | Back navigation |
| `FontUNIGBK.bin` | 470,252 bytes | System font |

## Development Tools

### 1. DWIN DGUS Tool (V7648+) - Official IDE
**Download:** https://www.dwin-global.com/tool-page/

**Features:**
- Full project management (DWIN_SET folder structure)
- Screen layout editor with drag-drop
- Touch region configuration
- Variable mapping
- BMP → BIN conversion
- Complete firmware compilation

**Output Files:**
- `13.bin` - Touch configuration
- `14.bin` - Display configuration
- `22.bin` - Variable initialization

**Driver:** XR21X141X Driver (included)

**Documentation:**
- [T5L DGUS Development Guide PDF](https://robu.in/wp-content/uploads/2021/12/T5L_DGUSII-Application-Development-Guide-20210929.pdf)

---

### 2. MKS TFT Image Convert - Simple Converter
**Download:** https://github.com/makerbase-mks/Software/tree/master/MKS%20TFT%20image%20convert

**Best For:**
- Quick icon replacement
- Batch BMP → BIN conversion
- Simple graphics updates without layout changes

**Usage:**
1. Open tool
2. Tools → Convert Image
3. Select BMP files
4. Output as `.bin` files

---

### 3. creality-dwin-lcd-customizer - Python Scripts
**Download:** https://github.com/capekoviroboti/creality-dwin-lcd-customizer

**Best For:**
- Command-line workflows
- Scripted batch processing
- Integration with build systems

---

### 4. Community Alternative: DGUS-reloaded
**Download:** https://github.com/Desuuuu/DGUS-reloaded

Complete replacement DWIN firmware for Marlin 2.0 - consider if you want a total UI overhaul.

## Customization Workflow

### Method 1: Simple Icon Replacement (Recommended to Start)

**Step 1: Prepare Graphics**
```
- Image editor: GIMP (free), Photoshop, Paint.NET
- Format: 24-bit BMP (RGB, no alpha)
- Color depth: 16.7M colors
- Compression: None
```

**Icon Dimensions:**
- Boot logo: 480x320 px
- Large buttons: 140x140 px
- Medium icons: 70x70 px
- Small icons: 32x32 px
- Navigation: 70x40 px

**Step 2: Convert to DWIN Format**
```bash
# Using MKS TFT Image Convert
1. Open tool
2. Load BMP files
3. Convert to BIN
4. Save with matching names (e.g., bmp_logo.bin)
```

**Step 3: Test on Printer**
```
1. Copy modified .bin files to Firmware/assets/
2. Copy entire assets/ folder to SD card root
3. Insert SD → Power on printer
4. Display will flash and update
5. Verify changes
```

---

### Method 2: Advanced Layout Customization

**Use DGUS Tool for:**
- Repositioning elements
- Adding new screens
- Changing touch regions
- Modifying screen flow
- Variable display logic

**Project Structure:**
```
DWIN_SET/
├── 13.bin          # Touch config (compiled)
├── 14.bin          # Display config (compiled)
├── 22.bin          # Variable init (compiled)
├── 0/              # Screen images folder
├── 1/              # Icon set 1
├── 2/              # Icon set 2
└── project.hmi     # DGUS project file
```

**Flashing:**
```
1. Copy entire DWIN_SET/ folder to SD root
2. Power on printer
3. Blue screen = update in progress
4. Wait for completion
5. Display will reboot
```

## Design Guidelines

### Color Schemes
Current theme uses:
- Primary: Blue/cyan accents
- Background: White/light gray
- Text: Black
- Alerts: Red
- Success: Green

**Recommendations:**
- High contrast for readability
- Consistent color language (red=stop, green=go)
- Consider colorblind users
- Test under different lighting

### Typography
- Current font: FontUNIGBK.bin (Chinese/English)
- Anti-aliased rendering
- Size varies by context
- Keep text minimal (space limited)

### UX Patterns
- Back button: Top-left or bottom-left
- Confirmation dialogs for destructive actions
- Progress indicators during operations
- Status always visible
- Touch targets: Minimum 40x40px

## Common Customization Ideas

### 1. Custom Boot Logo
Replace `bmp_logo.bin` with branded/personalized 480x320 image

### 2. Modern Icon Set
Redesign icons with:
- Flat design vs skeuomorphic
- Consistent stroke weights
- Modern color palette
- Better visual hierarchy

### 3. Dark Theme
Invert colors for:
- Reduced eye strain
- Better OLED longevity (if applicable)
- Modern aesthetic

### 4. Print Status Enhancement
Improve `bmp_printing.bin`:
- Larger temperature readouts
- Progress visualization
- Estimated time display
- Layer counter

### 5. Simplified Navigation
Reduce menu depth:
- Quick access tiles
- Flatten hierarchy
- Contextual menus

## Testing Checklist

Before releasing customized UI:

- [ ] Boot logo displays correctly
- [ ] All buttons respond to touch
- [ ] Touch regions align with graphics
- [ ] Text is legible at normal viewing distance
- [ ] Icons are recognizable at a glance
- [ ] Color scheme works in bright/dim lighting
- [ ] No visual glitches or artifacts
- [ ] All screens accessible
- [ ] Back navigation works from all screens
- [ ] Emergency stop is obvious and accessible
- [ ] Temperature readings visible and accurate
- [ ] Progress indicators function correctly
- [ ] SD card file list displays properly
- [ ] Print/pause/resume/stop buttons work
- [ ] Settings can be changed and saved

## Resources & Documentation

### Official DWIN Resources
- [DWIN Official Site](https://www.dwin-global.com/)
- [DGUS Tool Download](https://www.dwin-global.com/tool-page/)
- [T5L DGUS Development Guide](https://robu.in/wp-content/uploads/2021/12/T5L_DGUSII-Application-Development-Guide-20210929.pdf)

### Community Tutorials
- [DWIN Display Programming - Curious Scientist](https://curiousscientist.tech/blog/dwin-displays-and-their-programming)
- [Sebastien Andrivet's DGUS Guide](https://sebastien.andrivet.com/en/posts/dwin-mini-dgus-display-development-guide-non-official/)
- [MKS TFT Hardware Docs](https://github.com/makerbase-mks/MKS-TFT-Hardware)

### Community Projects
- [DGUS-reloaded](https://github.com/Desuuuu/DGUS-reloaded) - Alternative DWIN firmware
- [MKS TFT Examples](https://github.com/makerbase-mks/MKS-TFT) - Icon and theme examples
- [Creality DWIN Projects](https://github.com/juliandroid/DWIN_CR_10s_Pro)

### Forums & Support
- RepRap Forums - DWIN display section
- Reddit r/ender3 - Display mod posts
- MKS GitHub Issues - Technical questions
- Marlin Discord - #display channel

## Tips & Best Practices

### Do:
✅ Always backup original files before modifying
✅ Test on printer incrementally (don't change everything at once)
✅ Keep a version history of your custom graphics
✅ Document your changes
✅ Use consistent naming conventions
✅ Optimize file sizes where possible

### Don't:
❌ Change file names (display expects exact names)
❌ Use wrong color depth (must be 24-bit BMP)
❌ Skip testing touch regions after layout changes
❌ Forget to copy ALL files when updating display
❌ Use compressed BMP formats
❌ Exceed display resolution (480x320 max)

## Current Display Implementation

This firmware uses the **LGT_LCD_DW** implementation:
- Source: `Marlin/src/lcd/lgtdwlcd.h` and `.cpp`
- Serial communication via `LGT_SCR_DW` class
- Longer3D custom protocol
- Supports: T5, T5L, and JX screen models

**Key Functions:**
- `LGT_Send_Data_To_Screen()` - Send data to display
- `LGT_Analysis_DWIN_Screen_Cmd()` - Parse touch input
- `LGT_Get_MYSERIAL1_Cmd()` - Read serial commands

## Future Enhancement Ideas

- [ ] Add print statistics graphs
- [ ] Implement custom widgets
- [ ] Create animated transitions
- [ ] Add more printer status info
- [ ] Improve file browser (thumbnails?)
- [ ] Add calibration wizards with visual guides
- [ ] Network status display (if WiFi added)
- [ ] Multi-language support expansion
- [ ] Accessibility improvements
- [ ] Material library with profiles

---

**Note:** Keep this file private (add to .gitignore if needed). When you release a custom UI, update the main README with just the update instructions for users.
