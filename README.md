# Rstudio server with conda

```
# create the conda env
conda env create --force --name my_env -f environment.yml
# you can also use mamba
mamba env create --force --name my_env -f environment.yml

# ensure the path maps exactly to your own
# start docker
docker-compose up
```

error:

- UI is not coming up
- when executing the `/init` script inside the container I observe:

```
docker ps
CONTAINER ID   IMAGE                  COMMAND       CREATED          STATUS          PORTS                    NAMES
e741a92ac8b0   rocker/rstudio:4.2.1   "/init2.sh"   20 seconds ago   Up 20 seconds   0.0.0.0:8787->8787/tcp   rstudio-server-conda-rstudio-1

docker exec -ti e741a92ac8b0 bash

ERROR system error 71 (Protocol error) [description: Unable to parse version from R, version-info: , r-error: /usr/local/Caskroom/miniconda/base/envs/my_env/bin/R: 271: exec: /usr/local/Caskroom/miniconda/base/envs/my_env/lib/R/bin/exec/R: Exec format error
]; OCCURRED AT rstudio::core::Error rstudio::core::r_util::rVersion(const rstudio::core::FilePath&, const rstudio::core::FilePath&, const string&, const string&, const string&, const rstudio::core::FilePath&, std::__cxx11::string*) src/cpp/core/r_util/REnvironmentPosix.cpp:946; LOGGED FROM: bool rstudio::core::r_util::detectREnvironment(const rstudio::core::FilePath&, const rstudio::core::FilePath&, const string&, std::__cxx11::string*, std::__cxx11::string*, rstudio::core::r_util::EnvironmentVars*, std::__cxx11::string*, const string&, const string&, const rstudio::core::FilePath&) src/cpp/core/r_util/REnvironmentPosix.cpp:789
2022-07-29T12:23:15.070593Z [rserver] ERROR R shared library (/usr/local/Caskroom/miniconda/base/envs/my_env/lib/R/lib/libR.so) not found. If this is a custom build of R, was it built with the --enable-R-shlib option?; LOGGED FROM: bool rstudio::core::r_util::{anonymous}::validateREnvironment(const EnvironmentVars&, const rstudio::core::FilePath&, std::__cxx11::string*) src/cpp/core/r_util/REnvironmentPosix.cpp:372
R shared library (/usr/local/Caskroom/miniconda/base/envs/my_env/lib/R/lib/libR.so) not found. If this is a custom build of R, was it built with the --enable-R-shlib option?
```

How can this be fixed?

## solved by

https://github.com/conda-forge/r-base-feedstock/issues/215

using linux/linux and not mapping osx shared libraries to linux container
