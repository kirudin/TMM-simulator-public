# TMM Simulator User Manual

Version: v1.0.1  
Updated: 2026-07-05

This manual is for users who receive the packaged TMM Simulator ZIP release.
The app runs locally on the user's computer and opens a browser-based interface.

## 1. What This Program Does

TMM Simulator is a local research tool for optical multilayer calculations.

Main views:

- `Dispersion map`: energy-momentum reflection maps.
- `Isofrequency contour`: fixed-energy k-space maps, including energy sweep mode.
- `E-field profile`: layer-resolved field intensity versus depth at one selected point.
- `Material library`: dielectric-function browser and user material editor.
- `Guide`: short in-app notes on equations, axes, and export structure.

The plots show computed optical observables from an applied layer stack. A bright ridge or contour is a candidate optical feature, not by itself proof of a polariton mode.

## 2. Install And Launch

### macOS

1. Download the macOS ZIP release, for example `TMM-Simulator-Darwin-arm64-v1.0.1-YYYY-MM-DD.zip`.
2. Unzip it.
3. Open the extracted folder.
4. Double-click `Launch TMM Simulator.command`.
5. Keep the launcher window open while using the app.

If macOS blocks the launcher, right-click `Launch TMM Simulator.command`, choose `Open`, then confirm. The app writes logs to:

```text
~/Library/Logs/TMM Simulator/TMM-Simulator.log
```

### Windows

1. Download the Windows ZIP release, for example `TMM-Simulator-Windows-AMD64-v1.0.1-YYYY-MM-DD.zip`.
2. Unzip it.
3. Open the extracted folder.
4. Double-click the TMM Simulator executable.
5. If Windows SmartScreen appears, choose `More info` and then `Run anyway`.

The app starts a local server and opens a browser tab automatically. The browser address is usually similar to:

```text
http://127.0.0.1:8502/
```

If the default port is busy, the app uses another available local port.

## 3. First Dispersion Calculation

1. Open the `Dispersion map` tab.
2. In `Layer Stack`, choose materials for `Top`, `L1`, `L2`, ..., and `Sub`.
3. Set each finite layer thickness in nm. `Top` and `Sub` are semi-infinite.
4. Set `Angle (deg)` only when using anisotropic tensor materials in the 4x4 Berreman solver.
5. Click `Apply Stack`.
6. Choose `Calculation method`:
   - `Auto`: uses 4x4 when the applied stack needs tensor optics; otherwise uses 2x2.
   - `2x2 isotropic`: scalar transfer-matrix calculation.
   - `4x4 Berreman`: tensor Berreman calculation.
7. Set `Calculation Grid` energy and momentum ranges.
8. In `Display`, choose the plotted observable, axis units, colormap, and contrast options.
9. Click `Apply calculation`.

Recommended first test:

- Use a small grid such as `64 x 64`.
- Confirm units before increasing points.
- Increase resolution only after the plot looks physically reasonable.

## 4. Layer Stack

Layer labels are ordered from incident side to substrate side:

```text
Top / L1 / L2 / ... / Sub
```

Important conventions:

- Thickness is always entered in nm.
- Tensor rotation angle is entered in degrees.
- Material selection automatically fills the default thickness when the material has one.
- `Apply Stack` must be clicked before a calculation uses edited layer values.
- Stack presets save both the layer stack and the dispersion-map calculation grid.

Stack preset workflow:

1. Edit the stack and calculation grid.
2. Enter a preset name in `Stack preset`.
3. Click `Save`.
4. Use `Load` to restore the saved stack/grid later.
5. Use `Delete` to remove a saved preset.

Saved user materials and stack presets are stored in the user's app-data folder, outside the executable.

## 5. Display And Observables

Common observable presets:

- `Im(r_pp)`
- `Im(r_ps)`
- `Im(r_ss)`
- `Im(r_pp+r_ss)`

Use `Custom` to define an expression. The custom observable editor supports:

- Symbols: `r_pp`, `r_ps`, `r_sp`, `r_ss`
- Functions: `Im(x)`, `Re(x)`, `abs(x)`, `phase(x)`
- Operators: `+`, `-`, `*`, `/`, `**`, parentheses
- Examples: `Im(r_pp+r_ss)`, `abs(r_pp)**2`, `phase(r_pp)`

`Export Obs` controls which observables are written to CSV export. It can export multiple observables from the same calculated result without recomputing the map.

Axis display units:

- Energy axis: `eV`, `nm`, `cm^-1`
- Momentum axis: `um^-1`, `nm^-1`, `cm^-1`, `kx/k0`

In the browser UI these are displayed with typographic symbols such as `µm⁻¹`, `cm⁻¹`, and `kₓ/k₀`.

Contrast controls:

- `Auto contrast`: rescales the visible color range using the calculated data.
- `Log contrast`: applies logarithmic display contrast to emphasize weak features.
- `Reverse`: reverses the selected colormap.

These controls change the displayed image only. They do not change the stored calculation result.

## 6. Plot Interaction

Available plot interactions:

- Move the mouse over a plot to read x/y/value near the cursor.
- Hover crosshair lines mark the current cursor position.
- Drag an axis region to zoom the visible range.
- Use reset/full-view controls if available in the plot area.

Zooming changes only the view. It does not rerun the calculation.

## 7. Isofrequency Contour

Open the `Isofrequency contour` tab to compute reflection over a square k-space grid.

Grid convention:

```text
kx = -k max ... +k max
ky = -k max ... +k max
```

The grid is intentionally square. Set:

- Energy unit and energy value for a single IFC.
- `k max`
- `points`
- Method and stack as in the dispersion map.

Energy sweep mode calculates a stack of IFC slices across an energy range. This can be much heavier than a single IFC. Start with fewer energy points and fewer k-space points, then increase resolution after checking the result.

For anisotropic materials such as alpha-MoO3, the IFC shape depends on tensor components and in-plane rotation angle. If a known hyperbolic material looks circular, check:

- whether the 4x4 Berreman solver is selected or triggered by `Auto`
- whether the material is actually tensor-valued
- whether the energy range is in the Reststrahlen band
- whether the angle is in degrees
- whether the plotted observable is appropriate for the feature

## 8. E-field Profile

Open the `E-field profile` tab to calculate field intensity versus depth for one energy/momentum point.

Inputs:

- `E unit` and `E value`
- `k unit` and `k value`
- incident polarization
- depth grid resolution
- top-medium extent and substrate extent

`Use map center` copies the current dispersion-grid midpoint into the E-field point controls.

Displayed curves:

- `|E|²`
- `|Ex|²`
- `|Ey|²`
- `|Ez|²`

Depth convention:

- `z = 0` is the first interface from the top side.
- Negative z is the top medium.
- Positive z runs through the finite stack and into the substrate.

If the requested depth range is inconsistent with the layer stack, reduce the extent or check layer thicknesses.

## 9. Material Library

Open the `Material library` tab to inspect and edit dielectric functions.

Main tools:

- Category buttons filter the material list.
- `Material detail` shows metadata for the selected material.
- `Dielectric function` plots Re/Im epsilon versus the selected x axis.
- `Follow Display E axis` makes the library plot follow the energy-axis display unit.
- `CSV` exports the currently plotted dielectric function.
- `+ Add / update material` opens the user material editor.

User material models:

- `Constant`
- `Lorentz oscillator`
- `Drude`
- `User defined`

Epsilon types:

- `Isotropic`: one epsilon component is used for all axes.
- `Uniaxial`: in-plane `xx=yy` and out-of-plane `zz`.
- `Full 3x3`: independent diagonal `xx`, `yy`, and `zz` components.

For `User defined`, the app can load TXT, CSV, TSV, DAT, or XLSX dielectric tables. Check the detected axis unit and tensor columns before saving.

## 10. Export

After a calculation, use:

- `PNG`: downloads the currently displayed plot image.
- `CSV`: downloads a ZIP package containing `metadata.csv` plus one or more data CSV files.

CSV packages use matrix-style data.

Dispersion map export:

- ZIP name: `tmm_dispersion_export.zip`
- Metadata: `metadata.csv`
- Data: `dispersion_<observable>.csv`
- Rows: displayed energy axis
- Columns: displayed momentum axis
- Values: selected observable

IFC single export:

- ZIP name: `tmm_ifc_export.zip`
- Metadata: `metadata.csv`
- Data: `ifc_<observable>.csv`
- Rows: ky
- Columns: kx
- Values: selected observable

IFC energy sweep export:

- ZIP name: `tmm_ifc_energy_sweep_export.zip`
- Metadata: `metadata.csv`
- Data: `ifc_sweep_<observable>.csv`
- Rows: energy
- Columns: flattened k-space pixels
- Values: selected observable

Material epsilon export:

- CSV contains the selected x axis plus Re/Im epsilon components.

Metadata includes:

- export type
- data layout
- axis roles and units
- matrix shape
- selected observables
- layer stack structure from `Top` to `Sub`

## 11. Performance Guidelines

2x2 calculations are usually fast. 4x4 Berreman calculations are heavier because each grid point propagates tensor fields through the stack.

Practical starting points:

- Dispersion: `64 x 64` for quick checks, then increase.
- IFC single: `128 x 128` for inspection, then increase if needed.
- IFC sweep: keep both energy points and k-space points modest at first.

If a calculation is too slow:

- reduce grid points
- narrow the energy or momentum range
- use 2x2 only for scalar isotropic stacks
- avoid high-resolution IFC sweeps until the setup is validated

The progress indicator reports calculation progress as a percentage for heavier jobs.

## 12. Scientific Checks Before Interpreting A Mode

Before assigning a feature to a polariton mode, check:

- What was directly computed: reflection observable, field profile, or dielectric function.
- What is inferred: dispersion branch, IFC contour, confinement, or mode identity.
- Whether a simpler optical interference or cavity effect could explain the feature.
- Whether the result changes with grid resolution or axis units.
- Whether material parameters and layer thicknesses are correct.
- Whether tensor axes and angle conventions are correct.
- Whether substrate or top-medium effects dominate the response.

Claims should be treated carefully:

- Bright ridges are suggestive, not demonstrated modes.
- Hyperbolic polariton interpretation requires anisotropic dispersion behavior and exclusion of simpler explanations.
- Strong coupling claims require anticrossing, linewidth discussion, and an appropriate coupled-oscillator analysis.

## 13. Troubleshooting

### The browser says `Failed to fetch`

The backend server is probably not running or the launcher window was closed.

Try:

1. Close the browser tab.
2. Relaunch the app from the executable or launcher command.
3. Keep the launcher window open.
4. Check the log file if it still fails.

### The app opens on a different port

This is normal when the default port is already in use. Use the browser tab opened by the launcher.

### 4x4 results do not look anisotropic

Check:

- calculation method is `4x4 Berreman` or `Auto` with a tensor stack
- material epsilon type is `Uniaxial` or `Full 3x3`
- energy is in the expected spectral region
- layer angle is in degrees
- layer stack was applied after editing

### Material data looks wrong

Check:

- input axis unit: `eV`, `cm^-1`, or `nm`
- imported table columns
- default thickness in nm
- whether the material is isotropic, uniaxial, or full tensor

### CSV files look transposed or difficult to read

Open `metadata.csv` first. It defines the row axis, column axis, matrix shape, units, observables, and layer stack.

### User materials or stack presets disappeared after update

Packaged app user data is stored outside the executable:

- macOS: `~/Library/Application Support/TMM Simulator/`
- Windows: `%APPDATA%\TMM Simulator\`
- Linux: `$XDG_DATA_HOME/TMM Simulator/` or `~/.local/share/TMM Simulator/`

Back up that folder before replacing a release if custom materials or stack presets are important.
