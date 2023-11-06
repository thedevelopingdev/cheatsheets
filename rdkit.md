# RDKit

- [Greg Landrum's blog](https://greglandrum.github.io/rdkit-blog/)

## ACS1996 mode
- [source](https://www.rdkit.org/docs/source/rdkit.Chem.Draw.rdMolDraw2D.html#rdkit.Chem.Draw.rdMolDraw2D.SetACS1996Mode)
- 

```python
# dopts = d2d.drawOptions()

bondLineWidth = 0.6
scaleBondWidth = False
scalingFactor = 14.4 / meanBondLen
multipleBondOffset = 0.18
highlightBondWidthMultiplier = 32
setMonochromeMode # black and white
fixedFontSize = 10
additionalAtomLabelPadding = 0.066
fontFile # if it isn’t set already, then if RDBASE is set and the file exists, uses $RDBASE/Data/Fonts/FreeSans.ttf. Otherwise uses BuiltinRobotoRegular.
```