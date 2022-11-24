# cloudy-apec-combined
Cloudy generated models and their comparison with apec photon flux output.
![alt text]([https://github.com/[username]/[reponame]/blob/[branch]/image.jpg](https://github.com/RitaliG/cloudy-apec-combined/blob/main/comparison.png)?raw=true)![comparison](https://user-images.githubusercontent.com/59788464/203716185-759f0b7a-09a9-416b-8d38-dbdc8aa4d394.png)
![plot]([./directory_1/directory_2/.../directory_n/plot.png](https://github.com/[username]/[reponame]/blob/[branch]/image.jpg))

## How to get started? ###
# CLOUDY model generation
* Have the `cloudy_shared.exe` and `libcloudy.so` inthe working directory.

* Create an input condition file for generating the models, eg, `apec_compare.in` and save diffuse and total emission in <file_name>.lin, with params like:
  ```
  CMB redshift 0. 
  sphere
  radius 150 to 151 linear kiloparsec
  abundances "allen73.abn"
  metals 0.3 linear
  hden -4 log
  ## constant temperature, T=0.5 keV linear
  coronal, T=2.3e7 K linear
  stop zone 1
  iterate convergence
  age 1e9 years
  save continuum "temp-2.lin" units keV no isotropic
  save diffuse continuum "temp-2-emm.lin" units keV no isotropic
  ```
* Run the CLOUDY model generators for various params:
  >> ./cloudy_shared.exe -r apec_compare

* Do this for multiple temperature and metallicities for comparison.


# APEC model generation
If you have patomdb and apec installed, go ahead with the following steps:
* Load threeML conda environment:
  >> conda activate threeML

* Open the jupyter notebook and import the PYATOMDB database:
  `>> jupyter-notebook apec-test.ipynb
   >> 
* Initialise the APEC module and choose the abundances, temperature (kT in keV), normalisation (K in EM units), redshift, metallicity:
  ```
  from threeML import *
  modapec = APEC();  modapec.abundance_table='Allen'
  # input params
  modapec.kT.value       = 1.0   # keV temperature
  modapec.K.value        = 1e-2  # Normalization, proportional to emission measure
  modapec.redshift.value = 0.    # Source redshift
  modapec.abund.value    = 0.3   # The metal abundance of each element is set to 0.3 times the Solar abundance
  ```

* Create energy grid (sets the energy resolution you are sampling) and call modapec for corresponding photon flux:
  ```
  energies = np.logspace(-1., 1.5, 2000) # Set up the energy grid ## energy resoultion
  modapec(energies) # gives the output spectrum in the enrgy bins provided
  ```

__:bookmark:Dependencies:__ 
> - apec-test.ipynb &rarr; jupyter notebook to compare cloudy generated emission with APEC photon flux
> - apec_compare.in &rarr; input file for cloudy
> - apec_compare.in &rarr; output file for cloudy
> - <file_name>.lin &rarr; Variuous example files generated by CLOUDY (out-> $4 \pi \nu J_{\nu} (erg/s/cm^{-2})$ with different temperatures for fixed metallicity of 0.3 solar.
> - <file_name>-emm.lin &rarr; Corresponding outputs of $\nu (keV)$ vs $4 \pi \nu j_\nu (erg/s/cm^{-3})$.

__:mortar_board: Collaborators:__  Alankar Dutta, Ritali Ghosh, Prof. Prateek Sharma.
