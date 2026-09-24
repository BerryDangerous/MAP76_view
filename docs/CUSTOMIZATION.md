# Customization

## Custom Textures for Worldspaces

Custom map textures can replace the default map or provide maps for entirely new custom worldspaces.

### File Structure

Map configuration files must reside within the `Data/PrismaUI_F4/views/MAP76/assets/maps/` directory inside a folder named exactly after the target worldspace's **Editor ID** (in lowercase).

Examples:
- Commonwealth map configuration: `Data/PrismaUI_F4/views/MAP76/assets/maps/commonwealth/satellite8k.json`.
- Custom worldspace configuration: `Data/PrismaUI_F4/views/MAP76/assets/maps/appalachia/default.json`.

Map texture files (e.g., `.png`, `.svg`) must be placed in the corresponding folder.

### The JSON Configuration File

The root key of the JSON file represents the **Map Config ID**. This ID should be unique across all loaded mods. If two map configurations use the identical Map Config ID, the last one parsed will overwrite the previous configuration.

Example structure (Commonwealth Satellite 8K):

```json
{
    "Commonwealth_Satellite8K": {
        "worldspaceEditorID": "Commonwealth",
        "worldspaceID": 60,
        "mapName": "Satellite 8K",
        "texturePath": "assets/maps/commonwealth/satellite8k.png",
        "imageDimensions": {
            "width": 8192,
            "height": 8192
        },
        "cellBounds": {
            "nwCellX": -60,
            "nwCellY": 60,
            "seCellX": 60,
            "seCellY": -60
        }
    }
}
```

#### Configuration Options

- **`worldspaceEditorID`**: The Editor ID of the worldspace (e.g., `"Commonwealth"`, `"Appalachia"`).
- **`worldspaceID`**: The local FormID (in base 10) of the worldspace.
- **`mapName`**: A display name for the map configuration.
- **`texturePath`**: The relative path to the map texture image. Supported formats include `.png` and `.svg`. Note that `.dds` format is **not** supported.
- **`imageDimensions`**: The width and height of the map image in pixels.
- **`cellBounds`**: The bounding box of the cells in the game world that the map covers (`nwCellX`, `nwCellY`, `seCellX`, `seCellY`).
- **`imageGuttersPixels` (Optional)**: Defines padding around the actual map area in pixels. If not provided, the map defaults to the standard gutter percentages used by the Pip-Boy. This allows textures from existing mods to be easily extracted and reused without additional effort. Example:
```json
        "imageGuttersPixels": {
            "left": 0,
            "right": 0,
            "top": 0,
            "bottom": 0
        }
```

## Icon Overrides

The primary purpose of icon overrides is to expand the limited number of icon types provided by the engine. For example, it allows the integration of additional icons introduced in Fallout 76 that are necessary for certain map markers. 

The actual map marker data (such as location, name, and discovery state) originates from the plugin itself; the JSON configuration is used only to override the visual icon. Furthermore, override configurations do not need to be exhaustive. If a marker does not have an override specified in the JSON file, it will simply fall back to using its default icon type defined in the plugin.

While icon overrides can also be utilized to change the aesthetic style of existing icons, replacing the base icon image files directly is an alternative approach for stylistic changes. These two methods serve distinct structural purposes.

### File Structure

Icon override definitions are placed in JSON files located in `Data/PrismaUI_F4/views/MAP76/assets/icons/location/overrides/`. 
The filename is completely arbitrary. Because the target plugin is explicitly defined inside the file, a single JSON file can contain overrides for multiple different plugins simultaneously.

Example:
`Data/PrismaUI_F4/views/MAP76/assets/icons/location/overrides/my_custom_icons.json`

### The JSON Configuration File

The configuration utilizes the `markerOverrides` root object, followed by the specific plugin name (e.g., `Appalachia.esm`). Within the plugin object, an array of location objects maps local FormIDs to their corresponding icon names.

Example configuration:

```json
{
  "markerOverrides": {
    "Appalachia.esm": [
      {
        "localFormId": 16820,
        "icon": "power-plant"
      },
      {
        "localFormId": 20583,
        "icon": "bottling-plant_alt"
      }
    ]
  }
}
```

#### Configuration Properties

- **`markerOverrides`**: The root key declaring the file as an icon override configuration.
- **`"PluginName.esm"`**: The name of the plugin where the map markers are defined.
- **`localFormId`**: The local FormID (in **base 10**, not hexadecimal) of the map marker reference in the plugin.
- **`icon`**: The filename of the icon (excluding the extension) to be used for the map marker.

#### Conflict Resolution

Icon override JSON files are parsed alphabetically by filename. In the event of conflicting overrides targeting the same local FormID, the last override parsed will take precedence and win.
