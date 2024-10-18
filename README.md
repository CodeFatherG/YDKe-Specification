# YDKe URI Specification

YDKe is the export format used by many modern Yu-Gi-Oh! simulators.

## Format

```
ydke://<main>!<extra>!<side>!
```

- Three components: main deck, extra deck, and side deck, separated by `!`
- Each component is a base64 encoded string of Konami card passcodes
- Passcodes are stored as 32-bit little-endian values
    - It is likely that the endianess is dependant on the exporter architecture. Almost everything is LE now days almost standardising that this is enforced LE. If you intend your implementation to be BE compatible, you should test this and raise an issue.

## Examples

### Basic Example

1. `Blue-Eyes White Dragon` (passcode: 89631139)
2. EDOPro export: `ydke://o6lXBQ==!!!`
3. Components: `['o6lXBQ==', '', '']`
4. Decoded: `A3A95705` → Little-endian: `0x0557A9A3` (89631139)

### Multi-Component Example

```
ydke://o6lXBQbqRAQ=!SSyyAg==!rvTMAg==!
```

1. Components: `['o6lXBQbqRAQ=', 'SSyyAg==', 'rvTMAg==']`
2. Main deck:
   - `A3A9570506EA4404` → `[A3A95705, 06EA4404]`
   - `0x0557A9A4` (89631139): [Blue-Eyes White Dragon](https://yugioh.fandom.com/wiki/89631139)
   - `0x0444EA06` (71625222): [Time Wizard](https://yugioh.fandom.com/wiki/71625222)
3. Extra deck:
   - `492CB202` → `0x02B22C49` (45231177): [Flame Swordsman](https://yugioh.fandom.com/wiki/45231177)
4. Side deck:
   - `AEF4CC02` → `0x02CCF4AE` (46986414): [Dark Magician](https://yugioh.fandom.com/wiki/46986414)

**Note**: Passcode decoding process:
1. Base64 decode to hex
2. Convert to little-endian
3. Convert to decimal for final passcode
