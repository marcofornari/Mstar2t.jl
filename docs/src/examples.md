# Examples

Check [Usage](@ref) for examples on how to use the CLI and GUI version of m*2T.

The [examples](https://github.com/marcofornari/Mstar2t.jl/tree/main/examples) folder collects examples of the usage of m*2T as a Julia library.

## Constant relaxation time approximation CRTA

### Single-band model

```jldoctest
using Mstar2t
using Mstar2t: Scattering

# BAND STRUCTURE DEFINITION
m = [1.0, 1.0, 1.0, 0.0, 0.0, 0.0];
ϵ₀ = 0.0;
type = 1;   # conduction band
deg = 1;
band_1 = ParabBand(m,ϵ₀,type,deg);  # create the band
μ = collect(-.1:.01:.1);            # fermi level position
model = BandStructure(1,band_1,μ)   # build the band structure

T = collect(50:10:650);             # temperature
τ_form = Scattering.constant()      # relaxation time

# TENSORS COMPUTATION
σ = electrical_conductivity(model,T,τ_form);    # electrical conductivity
S = seebeck_coefficient(model,T,τ_form);        # seebeck coefficient
n = carrier_concentration(model,T,τ_form);      # carrier concentration
```

[More examples](https://github.com/marcofornari/Mstar2t.jl/tree/main/examples/CRTA/SingleBand)

### Bipolar transport

```jldoctest
using Mstar2t
using Mstar2t: Scattering

# BAND STRUCTURE DEFINITION
m_1 = [0.1, 0.1, 0.1, 0.0, 0.0, 0.0];
ϵ₀_1 = 1.0;
type_1 = 1;  # conduction band
deg_1 = 1;
band_1 = ParabBand(m_1,ϵ₀_1,type_1,deg_1);  # create the conduction band

m_2 = [1.0, 0.5, 0.5, 0.0, 0.0, 0.0];
ϵ₀_2 = collect(0.0:0.005:1.05);
type_2 = -1;    # valence band
deg_2 = 1;
band_2 = ParabBand(m_2,ϵ₀_2,type_2,deg_2);  # create the valence band

μ = 0.8;
model = BandStructure(2,[band_1,band_2],μ); # build the two-band structure

T = collect(50:50:700);         # temperature
τ_form = Scattering.constant(); # relaxation time

# TENSORS COMPUTATION
σ = electrical_conductivity(model,T,τ_form);    # electrical conductivity
S = seebeck_coefficient(model,T,τ_form);        # seebeck coefficient
n = carrier_concentration(model,T,τ_form);      # carrier concentration
```

[More examples](https://github.com/marcofornari/Mstar2t.jl/tree/main/examples/CRTA/BipolarTransport)

### Band convergence

```jldoctest
using Mstar2t
using Mstar2t: Scattering

# BAND STRUCTURE DEFINITION
m_1 = [1.0, 1.0, 1.0, 0.0, 0.0, 0.0];
ϵ₀_1 = 0.5;
type_1 = 1;
deg_1 = 1;
band_1 = ParabBand(m_1,ϵ₀_1,type_1,deg_1);   # create the conduction band

m_2 = [1.0, 1.0, 1.0, 0.0, 0.0, 0.0];
ϵ₀_2 = 0.0;
type_2 = -1;
deg_2 = 1;
band_2 = ParabBand(m_2,ϵ₀_2,type_2,deg_2);   # create the "fixed" valence band

m_3 = [1.0, 1.0, 1.0, 0.0, 0.0, 0.0];
ϵ₀_3 = collect(-0.5:0.001:0.5);
type_3 = -1;
deg_3 = 1;
band_3 = ParabBand(m_3,ϵ₀_3,type_3,deg_3);   # create the "moving" valence band

μ = -0.1;
model = BandStructure(3,[band_1,band_2,band_3],μ);   # build the three-band structure

T = collect(50:50:700);         # temperature
τ_form = Scattering.constant(); # relaxation time

# TENSORS COMPUTATION
σ = electrical_conductivity(model,T,τ_form);    # electrical conductivity
S = seebeck_coefficient(model,T,τ_form);        # seebeck coefficient
n = carrier_concentration(model,T,τ_form);      # carrier concentration
```

[More examples](https://github.com/marcofornari/Mstar2t.jl/tree/main/examples/CRTA/BandConvergence)

### Wiedemann-Franz law

```jldoctest
using Mstar2t
using Mstar2t: Scattering

# BAND STRUCTURE DEFINITION
m = [0.1, 1.0, 10.0, 0.0, 0.0, 0.0];
ϵ₀ = 0.0;
type = 1;
deg = 1;
band_1 = ParabBand(m,ϵ₀,type,deg);   # create the band

μ = collect(-0.2:0.0005:0.2);
model = BandStructure(1,band_1,μ);   # build the band structure

T = collect(50.:50:700);
τ_form = Scattering.constant();     # relaxation time

# TENSORS COMPUTATION
σ = electrical_conductivity(model,T,τ_form);    # electrical conductivity
K = thermal_conductivity(model,T,τ_form);       # thermal conductivity
L = lorenz_tensor(model,T,τ_form);              # Lorentz tensor

```

[More examples](https://github.com/marcofornari/Mstar2t.jl/tree/main/examples/CRTA/WF_law)

## Non-constant relaxation time approximation NCRTA

### Single-band model 

```jldoctest
using Mstar2t
using Mstar2t: Scattering


# BAND STRUCTURE DEFINITION
m = [0.5, 0.5, 0.5, 0.0, 0.0, 0.0];
ϵ₀ = 0.0;
type = -1;
deg = 1;
band_1 = ParabBand(m,ϵ₀,type,deg);   # create the band

μ = collect(-0.1:0.005:0.2);
model = BandStructure(1,band_1,μ);   # build the band structure

T = collect(51:10:650); # temperature

# impurity scattering
τ_im = 0.1;
A_im = .5;
γ_im = 0.2;
τ_form = Scattering.impurity(τ_im,A_im,γ=γ_im)

σ_nc = electrical_conductivity(model,T,τ_form); # electrical conductivity
S_nc = seebeck_coefficient(model,T,τ_form);     # seebeck coefficient
n_nc = carrier_concentration(model,T,τ_form);   # carrier concentration
```

[Full script](https://github.com/marcofornari/Mstar2t.jl/tree/main/examples/NCRTA/singleband.jl)

### Double-band model

```jldoctest
using Mstar2t
using Mstar2t: Scattering


# BAND STRUCTURE DEFINITION
m_1 = [.5, .5, .5, 0.0, 0.0, 0.0];
ϵ₀_1 = .3;
type_1 = 1;
deg_1 = 1;
band_1 = ParabBand(m_1,ϵ₀_1,type_1,deg_1)   # create the conduction band

m_2 = [1.0, 1.0, 1.0, 0.0, 0.0, 0.0];
ϵ₀_2 = .0;
type_2 = -1;
deg_2 = 1;
band_2 = ParabBand(m_2,ϵ₀_2,type_2,deg_2)   # create the valence band

μ = collect(.2:0.01:.4);
model = BandStructure(2,[band_1,band_2],μ)   # build the two-band structure

T = collect(51:10:650); # temperature
τ_form = Scattering.acoustic(model,T₀=180,μ_min=5,μ_max=5); # acoustic phonon scattering

σ_nc = electrical_conductivity(model,T,τ_form); # electrical conductivity
S_nc = seebeck_coefficient(model,T,τ_form);     # seebeck coefficient
n_nc = carrier_concentration(model,T,τ_form);   # carrier concentration
```

[Full script](https://github.com/marcofornari/Mstar2t.jl/tree/main/examples/NCRTA/twobands.jl)

### Three-band model

```jldoctest
using Mstar2t
using Mstar2t: Scattering


# BAND STRUCTURE DEFINITION
m_1 = [.8, .8, .8, 0.0, 0.0, 0.0];
ϵ₀_1 = .45;
type_1 = 1;
band_1 = ParabBand(m_1,ϵ₀_1,type_1,1);   # create the band

m_2 = [1., 1., 1., 0.0, 0.0, 0.0];
ϵ₀_2 = .2;
type_2 = -1;
band_2 = ParabBand(m_2,ϵ₀_2,type_2,1);   # create the band

m_3 = [.5, .5, .5, 0.0, 0.0, 0.0];
ϵ₀_3 = .3;
type_3 = -1;
band_3 = ParabBand(m_3,ϵ₀_3,type_3,1);   # create the band

μ = collect(.2:0.01:.6);
model = BandStructure(3,[band_1,band_2,band_3],μ);   # build the band structure

T = collect(150:50:650); # temperature
τ_form = Scattering.acoustic(model);     # acoustic phonon scattering

σ_nc = electrical_conductivity(model,T,τ_form); # electrical conductivity
S_nc = seebeck_coefficient(model,T,τ_form);     # seebeck coefficient
n_nc = carrier_concentration(model,T,τ_form);   # carrier concentration
```

[Full script](https://github.com/marcofornari/Mstar2t.jl/tree/main/examples/NCRTA/threebands.jl)

### Four-band model

```jldoctest
using Mstar2t
using Mstar2t: Scattering


# BAND STRUCTURE DEFINITION
m_1 = [.8, .8, .8, 0.0, 0.0, 0.0];
ϵ₀_1 = .5;
type_1 = 1;
band_1 = ParabBand(m_1,ϵ₀_1,type_1,1);

m_2 = [.5, .5, .5, 0.0, 0.0, 0.0];
ϵ₀_2 = .35;
type_2 = 1;
band_2 = ParabBand(m_2,ϵ₀_2,type_2,1);

m_3 = [1., 1., 1., 0.0, 0.0, 0.0];
ϵ₀_3 = .32;
type_3 = -1;
band_3 = ParabBand(m_3,ϵ₀_3,type_3,1);

m_4 = [.7, .7, .7, 0.0, 0.0, 0.0];
ϵ₀_4 = .25;
type_4 = -1;
band_4 = ParabBand(m_4,ϵ₀_4,type_4,1);

μ = collect(.2:0.01:.6);
model = BandStructure(4,[band_1,band_2,band_3,band_4],μ);   # build the band structure

T = collect(150:50:650);
τ_form = Scattering.acoustic(model);     # acoustic phonon scattering

σ_nc = electrical_conductivity(model,T,τ_form); # electrical conductivity
S_nc = seebeck_coefficient(model,T,τ_form);     # seebeck coefficient
n_nc = carrier_concentration(model,T,τ_form);   # carrier concentration
```

[Full script](https://github.com/marcofornari/Mstar2t.jl/tree/main/examples/NCRTA/fourbands.jl)

### Five-band model

```jldoctest
using Mstar2t
using Mstar2t: Scattering


# BAND STRUCTURE DEFINITION
m_1 = [.8, .8, .8, 0.0, 0.0, 0.0];
ϵ₀_1 = .5;
type_1 = 1;
band_1 = ParabBand(m_1,ϵ₀_1,type_1,1);

m_2 = [.5, .5, .5, 0.0, 0.0, 0.0];
ϵ₀_2 = .35;
type_2 = 1;
band_2 = ParabBand(m_2,ϵ₀_2,type_2,1);

m_3 = [1., 1., 1., 0.0, 0.0, 0.0];
ϵ₀_3 = .32;
type_3 = -1;
band_3 = ParabBand(m_3,ϵ₀_3,type_3,1);

m_4 = [.5, .5, .5, 0.0, 0.0, 0.0];
ϵ₀_4 = .25;
type_4 = -1;
band_4 = ParabBand(m_4,ϵ₀_4,type_4,1);

m_5 = [.7, .7, .7, 0.0, 0.0, 0.0];
ϵ₀_5 = .22;
type_5 = -1;
band_5 = ParabBand(m_5,ϵ₀_5,type_5,1);

μ = collect(.2:0.01:.6);
model = BandStructure(5,[band_1,band_2,band_3,band_4,band_5],μ);   # build the band structure

T = collect(150:50:650);
τ_form = Scattering.acoustic(model);     # acoustic phonon scattering

σ_nc = electrical_conductivity(model,T,τ_form); # electrical conductivity
S_nc = seebeck_coefficient(model,T,τ_form);     # seebeck coefficient
n_nc = carrier_concentration(model,T,τ_form);   # carrier concentration
```

[Full script](https://github.com/marcofornari/Mstar2t.jl/tree/main/examples/NCRTA/fivebands.jl)
