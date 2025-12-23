## The Core Problem

**OneDrive does NOT always keep files physically on your computer**, even if you can see them in File Explorer.
To save disk space, OneDrive uses a feature called **Files On-Demand**:

- The file **appears** in the folder
- But it is **not actually stored locally**
- It is only downloaded when you open it

This works fine for Word documents or PDFs,  
but it causes serious issues with **Obsidian**.

![[OneDrive states.png]]

> *Summary*: ***Your vault needs To be in Always Available state***
### ☁️ Cloud-only

- **Not available offline**
- Requires internet to download
- ❌ Not safe for Obsidian
### ✔️ “On this device” (outlined green check)

- File is **currently downloaded**
- **Available offline right now**
- ⚠️ OneDrive _may remove it later_ to save space so isn't safe for obsidian either
### ✔️✔️ “Always keep on this device” (solid green check)
- File is **permanently stored locally**
- **Always available offline**
- ✅ Safe and required (to ensure working without problems) for Obsidian
---
## Why This Is a Problem for Obsidian

Obsidian:
- Constantly scans the vault folder
- Expects **all files to exist physically on disk**
- Does NOT understand “cloud-only” files

If OneDrive decides to offload (passing from `On this device` to `Online-only` state) a file:
- Windows still shows the filename
- But the file is **not present locally**
- Obsidian interprets this as:

  > “The file has been deleted”
### Common consequences:
- Missing notes
- Broken links
- An empty or partially empty vault
- Changes not saving correctly

---
## What “Always keep on this device” Means

When you mark a folder as **Always keep on this device**, you are telling OneDrive:

> “This file must ALWAYS exist locally on my computer, so never offload it”

This means:
- The file is fully downloaded
- OneDrive will **never remove it from disk**
- Syncing still works normally

For Obsidian, this setting is **mandatory** to avoid future problems.

---

## What You Should Do (Step by Step)

1. Locate your Obsidian vault folder
2. Right-click the vault folder
3. Select **“Always keep on this device”**
4. Wait until OneDrive:
   - Downloads all files
   - Changes all icons to solid green checks

💡 Apply this to the **entire vault folder**, not individual files.

---

## What Happens If You **Don’t** Do This

Sooner or later:
- OneDrive will offload “unused” files that can happen to be some of your older notes
- Obsidian will think they were deleted
- You may lose:
  - Notes
  - Links
  - Plugin settings

---
## Quick Summary

- OneDrive may remove your local notes to liberate space
- Obsidian requires files to be **physically present**
- **Always keep on this device** = files are always local, never offload them.
- Without this → You're very likely to encounter problems in the future.
