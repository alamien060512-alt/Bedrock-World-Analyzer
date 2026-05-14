# Bedrock World Analyzer

A browser-based tool for inspecting and editing Minecraft Bedrock Edition world files (`.mcworld`). No uploads, no installs — everything runs locally in your browser.

---

## Features

### World Overview
- Displays world name, game mode, and difficulty at a glance
- Shows core stats: day number, in-game time, cheats status, storage version, education edition flag, and last played timestamp
- Seed display with one-click copy and a direct link to [Chunkbase](https://www.chunkbase.com) for seed mapping
- Player last-known position (X/Y/Z) or world spawn coordinates if no player data is present
- Game rules panel listing boolean rules stored in `level.dat`

### NBT Editor
- Full interactive tree view of `level.dat` NBT data
- **Edit values** inline — type-validated inputs (integers, floats, strings, bytes)
- **Rename keys** by clicking any key label
- **Delete fields** with a confirmation prompt
- **Add new fields** to the root compound or any nested compound tag, with type selection (Byte, Short, Int, Long, Float, Double, String)
- Expand / Collapse All for navigating deeply nested data
- Real-time key search with match highlighting
- Unsaved changes indicator

### Save & Export
- Repacks the edited `level.dat` back into the original `.mcworld` ZIP
- Downloads as `worldname_edited.mcworld`, ready to import back into Minecraft

### Archive Inspector
- Lists all files inside the `.mcworld` archive with file sizes and type badges (NBT, DB, JSON, PNG, etc.)

---

## How to Use

1. **Export your world** from Minecraft Bedrock Edition:
   `Settings → Storage → Export World`
   This saves a `.mcworld` file to your device.

2. **Open the tool** and either drag & drop the `.mcworld` file onto the drop zone, or click **Browse File** to pick it.

3. **Review the parsed info** — world stats, seed, and player position are shown automatically.

4. **Edit NBT data** in the editor below:
   - Click ✏ to edit a value
   - Click a key name to rename it
   - Click 🗑 to delete a field
   - Use the **+** button on any compound to add a child field
   - Use the bottom bar to add root-level fields

5. **Save your changes** by clicking **💾 Save & Download**. The tool repacks everything into a new `.mcworld` file.

6. **Import back into Minecraft** by opening the downloaded file — Bedrock will import it automatically.

---

## Supported NBT Tag Types

| Tag | Type | Notes |
|-----|------|-------|
| `TAG_Byte` (1) | Integer | Editable; shown as 1/0 toggle for boolean-style fields |
| `TAG_Short` (2) | Integer | Editable inline |
| `TAG_Int` (3) | Integer | Editable inline |
| `TAG_Long` (4) | Integer | Editable inline |
| `TAG_Float` (5) | Float | Editable inline |
| `TAG_Double` (6) | Float | Editable inline |
| `TAG_String` (8) | String | Editable inline |
| `TAG_Compound` (10) | Nested object | Expandable; supports adding child fields |
| `TAG_List` (9) | Typed list | Expandable; read-only items (editing list elements coming soon) |
| `TAG_Byte_Array` (7) | Binary | Displayed as byte count; read-only |
| `TAG_Int_Array` (11) | Int list | Displayed as item count; read-only |
| `TAG_Long_Array` (12) | Long list | Displayed as item count; read-only |

---

## Technical Notes

- `.mcworld` files are ZIP archives containing a `level.dat` file and a LevelDB database directory
- `level.dat` uses **Bedrock-specific little-endian NBT** with an 8-byte header (4-byte version + 4-byte length)
- Parsing and writing is done entirely in JavaScript with no server-side code
- ZIP handling uses [JSZip](https://stuk.github.io/jszip/)
- The writer reconstructs a valid Bedrock NBT binary from the in-memory object tree and repacks it into the original ZIP

---

## Limitations

- The LevelDB chunk database (the `db/` folder) is not parsed — only `level.dat` is editable
- List tag item editing is not yet supported (items are displayed read-only)
- Binary array tags (ByteArray, IntArray, LongArray) are displayed but not editable
- Very large worlds may be slow to load due to browser memory constraints

---

## Privacy

All processing happens in your browser. No files are uploaded to any server. No data is collected or transmitted.
