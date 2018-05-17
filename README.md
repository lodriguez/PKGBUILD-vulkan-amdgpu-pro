# amdgpu-pro-vulkan-installer

This PKGBUILD installs amdgpu-pro-vulkan and lib32-amdgpu-pro-vulkan next to RADV

You can use it with the VK_ICD_FILENAMES environmental variable.

VK_ICD_FILENAMES=/usr/share/vulkan/icd.d/amd_icd64.json wine game-x86-64.exe 
VK_ICD_FILENAMES=/usr/share/vulkan/icd.d/amd_icd32.json wine game-i386.exe
