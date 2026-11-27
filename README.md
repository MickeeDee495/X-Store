#### Summary

This pull request integrates ~5x faster native Games on Demand (GOD) conversion, Original Xbox (00005000) container support, automatic NXE Storage title/icon embedding, and a 3-stage resumable download/decompression/conversion queue directly into **X-Store**. 

#### Key Improvements

* **~5x Faster Native GOD Conversion (iso2god.cpp):** Streams 4096-byte SVOD blocks and MHT/SHT hash tables directly from disc images and extracted folders, eliminating intermediate ISO copies and speeding up large Xbox 360 and Original Xbox conversions.
* **Native Original Xbox (00005000) Support:** Parses default.xbe (XBEH) headers, builds 00005000 LIVE containers, and decodes $$XTIMAGE / $$XSIMAGE (XPR0 DXT1) artwork into 64x64 NXE Storage PNG icons.
* **Automatic NXE Storage Titles & 64x64 Icons:** 

  * Extracts titles and 64x64 PNG icons directly on-console from retail XEX2 executables (AES-128-CBC decryption + multi-block LZX XDBF scanning), nxeart archives, and icon.dds (BGRA8888, DXT1, DXT3, DXT5 mipmaps via XGSetTextureHeader).
  * Falls back automatically to existing profile saves (00000001), DLC (00000002), or Title Updates (000B0000), and lists any edge-case title needing save data on the completion screen.
* **3-Stage Resumable Queue (store_queue_persist.h):** Saves queue progress across download, archive decompression, and GOD conversion stages so interrupted batches resume cleanly without restarting from scratch.
* **Completion Summary Screen:** Keeps **X-Store** open after batch completion (Press A to return to X-Store menu, or B to exit) instead of exiting immediately.

#### Testing

* Verified on retail Xbox 360 hardware across multi-gigabyte Xbox 360 (00007000) and Original Xbox (00005000) titles, confirming valid LIVE signatures, SHA-1 hashes, 0x411/0x1691 UTF-16 Big-Endian titles, and 64x64 PNG icons in NXE Storage.
