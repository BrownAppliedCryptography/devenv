# CS 1515: development environment

This repo contains a minimal dev environment setup for CS 1515. In
particular, it provides the scripts to pull or create the course Docker
containers.

## Getting started

```
# 0. Download Docker (follow instructions).

# 1. Setup Docker
./docker/cs1515-setup-docker

# 2. start development environment
./cs1515-run-docker
```

For detailed setup instructions, refer to our Lab 0 setup guide!

## Debugging Installation of `libscapi`

Basically, many things will break on ARM / M1 Mac so I recommend doing this on an Intel device if at all possible. AMD should also work; it depends on the processor and the amount of SIMD implemented on it.

- **unrecognized assembly instruction in asm** I've already updated the thing to work w/ our docker on m1. If it doesn't work, add `set(ARCH "A64")` to the following code snippet in `libscapi/build/OTExtensionEncrypto/extern/ENCRYPTO_utils/extern/relic/src/CMakeLists.txt` or `libscapi/lib/OTExtensionEncrypto/extern/ENCRYPTO_utils/extern/relic/src/CMakeLists.txt`:

```
set(ARCH "A64") # Add this line, changing out A64 for your arch; X86, X64, A64, ARM
if (ARCH)
	string(TOLOWER ${ARCH} ARCH_PATH)
	if (EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/arch/relic_arch_${ARCH_PATH}.c")
		list(APPEND CORE_SRCS "arch/relic_arch_${ARCH_PATH}.c")
	endif(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/arch/relic_arch_${ARCH_PATH}.c")
else()
	list(APPEND CORE_SRCS "arch/relic_arch_none.c")
endif()
```

- **missing file xxmintrim.h** just delete `libscapi/build/OTExtensionEncrypto/extern/ENCRYPTO_utils/extern/relic/src/CMakeFiles/relic_s.dir/low/curve2251-sse/` or or `libscapi/lib/OTExtensionEncrypto/extern/ENCRYPTO_utils/extern/relic/src/CMakeFiles/relic_s.dir/low/curve2251-sse/`. Is this safe? Probably not.

## Acknowledgements

This setup is a modified version of the setup used by
[Harvard's CS61](https://cs61.seas.harvard.edu/site/2021/) and reused
with permission.
