# Project Description
A Global Circulation Model (GCM) is used to simulate the atmospheric dynamics of a tidally locked planet at different rotation rates. Future comparisons may be made with observations of transmission spectra or thermal phase curves. To simulate tidally locked exoplanets and explore the effects of varying rotation rates on atmospheric jet formation, the script from Schneider and Liu (2009) within the ISCA framework was used.

# Parameters of Exoplanet
Tidally locked hot Jupiters are gas giant exoplanets that orbit very close to their host stars, resulting in synchronous rotation where the planet’s orbital period matches its rotational period.

HD 189733b was chosen as an [example](https://science.nasa.gov/exoplanet-catalog/hd-189733-b/), because it is well-studied, with detected atmospheric features.

| Parameter name | Parameter value | Units |
| --- | --- | --- |
| Orbital Distance | $\sim 0.031$ | $AU$ |
| Rotation period (assuming tidal locking) | $2.22$ | $days$ |
| Orbital Period | $\sim 2.22$ | $days$ |
| Radius | $1.138 \ R_{Jupiter}$ $\approx 8.2 * 10^7$ | $m$ |
| Gravity | $21.4$ | $m/s^2$ |
| Equilibrium temperature | $\sim1200$ | $K$ |
| Estimated solar constant (stellar flux received) | $\sim2300$ | $W/m^2$ |


# Changes in Script
Sections modified:
- Two_stream_gray_rad_nml
- Constants_nml
- Main_nml
- Diagnostics for time and necessary parameters

Changes in code:
```python
# giant_planet_tidal_locking.py
	...

	'main_nml': {
     'days'   : 365,
     'hours'  : 0,
     'minutes': 0,
     'seconds': 0,
     'dt_atmos':1800,
     'current_date' : [1,1,1,0,0,0],
     'calendar' : 'no_calendar'
     },

	...

	'two_stream_gray_rad_nml': {
        'rad_scheme': 'Schneider', #Use the Schneider & Liu option for the grey scheme
        'do_seasonal': False,               #Don't use seasonally-varying radiation 
        'atm_abs': 0.2,                      # default: 0.0
        'solar_constant': 2300,  # Estimated stellar flux in W/m²
        'diabatic_acce':  1.0, # Parameter to artificially accelerate the diabatic processes during spinup. 1.0 performs no such acceleration, >1.0 performs acceleration        
    },

	...

	#Set HD 189733b constants
	'constants_nml': {
	
        'radius': 8.2e7,  # HD 189733b radius in meters
        'grav': 21.4,  # Surface gravity in m/s²
        'omega': 3.27e-5,  # Rotational angular velocity (tidal locking)
        'orbital_period': 2.22 * 86400,  # Orbital period in seconds
        'PSTD': 3.0e+06,  # Standard pressure
        'PSTD_MKS': 3.0e+05,
        'rdgas': 3605.38,  # Specific gas constant for hydrogen-rich atmospheres
    },

	...

```


# Experiment
To simulate tidally locked planet using the ISCA model, I set the planet’s rotational period equal to its orbital period. This ensures that the same hemisphere always faces the star.

Planetary Rotation Rate:

$\omega = \frac{2\pi}{orbital period}$,

$\omega = \frac{2\pi}{191,808} \approx 3.27 \times 10^{-5} \text{ rad/s}$

The model will run for $1$ year, with output every $30$ days to capture the dynamics. Then, I will adjust the planet’s rotation rate by speeding it up and slowing it down to explore the impact of varying rotation rates on atmospheric jet formation. The expectation is that with faster rotation, the Coriolis force will be stronger, leading to multiple zonal jets and weaker day-night heat transport. Slower rotation should lead to a more global circulation with stronger equatorial winds carrying heat toward the night side.

To estimate whether the circulation is dominated by Coriolis effects or large-scale flows, the Rossby number ($Ro$) can be used to analyze the data output:

$Ro = \frac{U}{\Omega L}$,

where $U$ is the velocity of the flow, $\Omega$ is the planetary rotation rate, $L$ is the characteristic length scale. Usually a high $Ro$ corresponds to slow rotation, and a low $Ro$ corresponds to fast rotation.
