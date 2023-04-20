```@meta
CurrentModule = Mstar2t
DocTestSetup = quote
    using Mstar2t
    using .Utils
end
```

# Documentation

## Types

```@docs
Mstar2t.Utils.ParabBand
Mstar2t.Utils.BandStructure
```

## Functions

```@docs
Mstar2t.Transport.electrical_conductivity
Mstar2t.Transport.seebeck_coefficient
Mstar2t.Transport.carrier_concentration
Mstar2t.Transport.thermal_conductivity
Mstar2t.Transport.lorenz_tensor
Mstar2t.Transport.Ln

Mstar2t.Scattering.constant
Mstar2t.Scattering.T_fun
Mstar2t.Scattering.ET_fun
Mstar2t.Scattering.impurity
Mstar2t.Scattering.acoustic
Mstar2t.Scattering.matthiessen

Mstar2t.Plot.plot
Mstar2t.Plot.plot_bandstructure
Mstar2t.Plot.plot_τ
Mstar2t.Plot.savefig

Mstar2t.Utils.savedata
```
