# PhoneToyRemote V4

Source for the release DLL. One BepInEx IL2CPP plugin, two source files:

```
src/Plugin.cs               server, pairing, toy control, the audio/volume law
src/Ui.cs                   the whole phone page (HTML+CSS+JS) as one const
src/PhoneToyRemote.csproj   net6.0, references the game's BepInEx + interop
src/Properties/AssemblyInfo.cs
tests/test_page.py          runs the page JS in QuickJS against a stub DOM and
                            checks it against a port of the game-side maths
tests/prelude.js            stub DOM
tests/driver.js             the scenarios the test drives
tools/make-release.py       builds both zips from a working install
```

## Build

```
dotnet build src/PhoneToyRemote.csproj -c Release
copy src\bin\Release\PhoneToyRemote.dll <game>\BepInEx\plugins\
```

The csproj HintPaths point at a SecretFlasherManaka install (BepInEx\core and
BepInEx\interop); edit them if yours lives somewhere else.

## Test

```
pip install quickjs
python tests/test_page.py
```

It pulls the page out of src/Ui.cs, runs the real JS in QuickJS and checks:

- the pattern envelope against a port of the C# maths (384 samples)
- the piston level mapping against the game's own bands (101 values)
- 11 behaviour cases: idle, vibrator only, piston only (hold and gap), both
  toys with different strengths AND different patterns, linked, rail drags,
  vibrator off (graph must stay drawn and flat), STOP

## Design notes

**Clocks.** Each toy keeps its own pattern start time (`_vibeStartedUtc`,
`_pistonStartedUtc`). A pattern change and an off->on reset that toy's clock and
nothing else; intensity changes reset nothing, so dragging never restarts a
pattern. The page mirrors it with `vEpoch` / `pEpoch`. That is what keeps the
graph and the toy in the same phase - the bug this release exists to fix was a
single shared clock that the plugin restarted while the page kept its own.

**Audio.** `SetVibratorAudio(percent)` picks the band through
`VibrationLevelHysteresis` (47/54 margins, so a fast drag cannot flap the game's
loops) and fades the gain of the playing loop towards its target over a
*distance-scaled* time: a nudge fades in ~0.22s, a 30->90 slam takes ~0.6s.
Both the `SmoothFloat` and the `AudioSource` are written each frame.

**The page is one string.** `Ui.Html` is the single source of truth and the
server answers with `Html(Ui.Html)` - keep any page edit inside Ui.cs or it
will not be served.

18+ only.
