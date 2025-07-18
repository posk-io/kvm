# Changelog

## Upcoming Release

- Plumb through KVM_CAP_X2APIC_API as X2ApicApi cap.

## v0.23.0

### Added

- [[#322]](https://github.com/rust-vmm/kvm/pull/322) Added
  `VcpuFd::nested_state()`and `VcpuFd::set_nested_state()` to work with nested
  KVM state. Only works on `x86`. The helper type `KvmNestedStateBuffer`makes
  these new functions easily usable.

### Changed

- Upgrade `kvm-bindings` to `v0.13.0`

## v0.22.0

### Changed

- [[#324]](https://github.com/rust-vmm/kvm/pull/324) Upgrade kvm-bindings to v0.12.0
- [[#323]](https://github.com/rust-vmm/kvm/pull/323) Upgrade vmm-sys-util to v0.14.0

## v0.21.0

### Added

- [[#310](https://github.com/rust-vmm/kvm/pull/310)]: Added support for
  `KVM_CAP_XSAVE2` and the `KVM_GET_XSAVE2` ioctl.

### Changed

- [[#310](https://github.com/rust-vmm/kvm/pull/310)]: Changed `set_xsave()`
  `unsafe` because the C `kvm_xsave` struct was extended to have a flexible
  array member (FAM) in the end in Linux 5.16 and `KVM_SET_XSAVE` may copy data
  beyond the traditional size (i.e. 4096 bytes). If any features are enabled
  dynamically on Linux 5.16+, it is recommended to use `set_xsave2()` instead.
- [[#325]](https://github.com/rust-vmm/kvm/pull/325)]: Changed
  `set_gsi_routing()` to use the newly created `KvmIrqRouting` type which is a
  `FamStruct` wrapper over `kvm_irq_routing`. This way we can safely call the
  `ioctl` without relying on the caller making sure the memory layout of
  `kvm_irq_routing` is sane.

## v0.20.0

### Added

- [[#288](https://github.com/rust-vmm/kvm-ioctls/pull/288)]: Introduce `Cap::GuestMemfd`, `Cap::MemoryAttributes` and
   `Cap::UserMemory2` capabilities enum variants for use with `VmFd::check_extension`.
- [[#288](https://github.com/rust-vmm/kvm-ioctls/pull/288)]: Introduce `VmFd::check_extension_raw` and `VmFd::check_extension_int` to allow `KVM_CHECK_EXTENSION` to return integer.

### Changed

- [[#305](https://github.com/rust-vmm/kvm/pull/305)]: Updated kvm-bindings to 0.11.0.

### Fixed

- [[#298](https://github.com/rust-vmm/kvm/pull/298)]: Fixed incorrect usage of `ioctl_wit_ref` in the
  `create_device` method. Replace it with `ioctl_wit_mut_ref` as the passed parameter may be mutated by the
  ioctl.

## v0.19.0

### Added

- [[#275](https://github.com/rust-vmm/kvm-ioctls/pull/275)]: Introduce `riscv64` ioctls.

### Removed

- [[#289](https://github.com/rust-vmm/kvm-ioctls/pull/289)]: Drop `x86` 32-bit
  and `arm` 32-bit support.

### Changed

- [[#273](https://github.com/rust-vmm/kvm-ioctls/pull/273)]: `DeviceFd::get_device_attr` is now
  marked as unsafe.
- [[#277](https://github.com/rust-vmm/kvm-ioctls/pull/277)]: Updated kvm-bindings to 0.9.1.

## v0.18.0

### Added

- [[#264](https://github.com/rust-vmm/kvm-ioctls/pull/264)]: Added `KVM_SET_USER_MEMORY_REGION2`,
  `KVM_CREATE_GUEST_MEMFD` and `KVM_SET_MEMORY_ATTRIBUTES` ioctls.
- [[#267](https://github.com/rust-vmm/kvm-ioctls/pull/267)]: Added `HypercallExit` field to
  `VcpuExit::Hypercall` and added `ExitHypercall` to `Cap`.
- [[#270](https://github.com/rust-vmm/kvm-ioctls/pull/270)]: Added `MemoryFaultInfo` to `Cap` and
  propagated `MemoryFault` exit reason in `KVM_RUN`.

## v0.17.0

### Changed

- [[#255](https://github.com/rust-vmm/kvm-ioctls/issues/255)]: Fixed a
  soundness issue when accessing the `kvm_run` struct. `VcpuFd::run()` and
  `VcpuFd::set_kvm_immediate_exit()` now take `&mut self` as a consequence.
- [[#260](https://github.com/rust-vmm/kvm-ioctls/pull/260)]: Updated kvm-bindings to 0.8.0.

## v0.16.0

### Added
- [[#242](https://github.com/rust-vmm/kvm-ioctls/pull/242)] x86: add support
  for SMI injection via `Vcpu::smi()` (`KVM_SMI` ioctl).
- [[#241](https://github.com/rust-vmm/kvm-ioctls/pull/241)] Add support for
  userspace MSR handling.
- [[#246](https://github.com/rust-vmm/kvm-ioctls/pull/246)] Add support for
  userspace NMI injection (`KVM_NMI` ioctl).
- [[#244](https://github.com/rust-vmm/kvm-ioctls/pull/244)] add support for
  coalesced MMIO (`KVM_CAP_COALESCED_MMIO` / `KVM_CAP_COALESCED_PIO`)

### Changed
- [[#234](https://github.com/rust-vmm/kvm-ioctls/issues/234)] vcpu: export
reg_size as a public method.
- [[#243](https://github.com/rust-vmm/kvm-ioctls/pull/243)] derived the `Copy`
  trait for `IoEventAddress` and `NoDatamatch`.

## v0.15.0

### Added
- [[#230](https://github.com/rust-vmm/kvm-ioctls/pull/230)] Added
  `check_extension_raw` method to use raw integer values instead
  of `Cap` enum.
- [[#228](https://github.com/rust-vmm/kvm-ioctls/pull/228)] arm64: add
support for vCPU SVE feature.
- [[#219](https://github.com/rust-vmm/kvm-ioctls/pull/226)] Add `Cap::ArmPtrAuthAddress`
  and `Cap::ArmPtrAuthGeneric` capabilities.

## v0.14.0

### Added

- [[#219](https://github.com/rust-vmm/kvm-ioctls/pull/219)] Support for
  `KVM_GET_MSR_FEATURE_INDEX_LIST` and `KVM_GET_MSRS` system ioctls.
- [[#221](https://github.com/rust-vmm/kvm-ioctls/pull/221)] Add
  `Cap::ArmPmuV3`.

### Changed

- [[#223](https://github.com/rust-vmm/kvm-ioctls/pull/223)] aarch64:
  Updated `get/set_one_reg` to support different registers sizes through
  byte slices.

## v0.13.0

### Added
- [[#213](https://github.com/rust-vmm/kvm-ioctls/pull/213)] Add `Kvm::new_with_path()`
  and `Kvm::open_with_cloexec_at()` to allow using kvm device files other than
  `/dev/kvm`.

## v0.12.0

### Added

- [[#187](https://github.com/rust-vmm/kvm-ioctls/pull/187)] Support for
  `KVM_SET_IDENTITY_MAP_ADDR`
- Derive Debug for all exported structs and enums
- [[#189](https://github.com/rust-vmm/kvm-ioctls/pull/189)] Expose `KVM_SET_` and
  `KVM_HAS_DEVICE_ATTR` for vcpu
- [[#191](https://github.com/rust-vmm/kvm-ioctls/pull/191)] Add `KVM_TRANSLATE` support and
  the `translate_gva` function that translates guest virtual address to the physical address
- [[#190](https://github.com/rust-vmm/kvm-ioctls/pull/190)] Enable usage of `sync_regs`
  to allow bulk getting and setting of general purpose registers, reducing the number of
  ioctls needed.
- [[#198](https://github.com/rust-vmm/kvm-ioctls/pull/198)] Return details about
  `KVM_EXIT_FAIL_ENTRY` in vCPU run
- [[#199](https://github.com/rust-vmm/kvm-ioctls/pull/199)] Add `register_irqfd_with_resample`
  so that `irqfd` + `resaplefd` can be registered through `KVM_IRQFD`
- [[#202](https://github.com/rust-vmm/kvm-ioctls/pull/202)] Add `KVM_CAP_GUEST_DEBUG_HVM_DPS/WPS`
- [[#202](https://github.com/rust-vmm/kvm-ioctls/pull/202)] Added `check_extension_int`
  which allows checking the capabilities that return numbers instead of booleans

### Changed

- Updated vmm-sys-util to 0.11.0
- Updated kvm-bindings to 0.6.0
- Upgraded to rust 2021 edition
- Switched to specifying dependencies using caret requirements
  instead of comparision requirements
- [[#195](https://github.com/rust-vmm/kvm-ioctls/pull/195)] Do not panic on unsupported
  `KVM_EXIT` reason
- [[#196](https://github.com/rust-vmm/kvm-ioctls/pull/196)] Expose a mutable reference
  to the `kvm_run` structure to allow proper handling of unsupported exit reasons
- [[#200](https://github.com/rust-vmm/kvm-ioctls/pull/200)] Fix wrong `target_arch` gate
  preventing `set_guest_debug` from being exported on ARM
- [[#206](https://github.com/rust-vmm/kvm-ioctls/pull/206)] use `u128` in `get/set_on_reg`

## v0.11.0

### Added
- [[#178](https://github.com/rust-vmm/kvm-ioctls/pull/178)] Support for the AMD
  Security Encrypted Virtualization (SEV) through the following VM ioctls:
  `encrypt_op`, `encrypt_op_sev`, `register_enc_memory_region` and
   `unregister_enc_memory_region`.
- [[#184](https://github.com/rust-vmm/kvm-ioctls/pull/184)] `DeviceFd` now
  derives `Debug`.

## v0.10.0

### Changed
- Now depends on kvm-bindings >=0.5.0 which replaced the v4.20 KVM bindings
  with the v5.13 ones.
- Updated `VcpuExit::Debug` to return architecture specific information for the
  debug event.

## v0.9.0

### Added
- Support for accessing and controlling the Time Stamp Counter on x86 platforms
  through the `get_tsc_khz` and `set_tsc_khz` functions.

### Changed
- Updated `create_vm` on `aarch64` to create a VM fd from the KVM fd using the
  host's maximum IPA size.

## v0.8.0

### Added
- Support for specifying VM type (an opaque platform and architecture specific
  constant) when creating a VM (`KVM_CREATE_VM` ioctl) via the
`Kvm::create_vm_with_type` function.

### Changed
- Now depends on kvm-bindings >=0.4.0 to support use of a newer vmm-sys-utils
  dependency.

## v0.7.0

### Added
- Support for the system API that returns the maximum allowed vCPU ID
  (`KVM_CAP_MAX_VCPU_ID`).
- Support for `KVM_MEMORY_ENCRYPT_OP`.

### Fixed
- [[#119](https://github.com/rust-vmm/kvm-ioctls/issues/119)]: Disallow invalid
  number of cpuid entries to be passed to `get_supported_cpuid` and
  `get_emulated_cpuid`.

### Changed
- [[#123](https://github.com/rust-vmm/kvm-ioctls/issues/123)]: Updated
  `create_vcpu` to use `u64` as the parameter for the number of vCPUs.

## v0.6.0

### Added
- Support for the vcpu ioctls: `KVM_SET_GUEST_DEBUG`, `KVM_KVMCLOCK_CTRL`, and
  `KVM_GET_REG_LIST`.
- Support for the vm ioctl `KVM_GET_DEVICE_ATTR`.
- Support for the device ioctl `KVM_HAS_DEVICE_ATTR`.
- Support for `VcpuExit::Debug`.
- Support for enabling vcpu capabilities using `Vcpu::enable_cap`.
- Support for checking Hyper-V (`HypervSynic` and `HypervSynic2`), MSI
  (`MsiDevid`), and IPA Size (`ArmVmIPASize`) capabilities.
  using `kvm.check_extension`.
- Support for checking the VM capabilities via `Vm::check_extension`.
- Create a VM with flexible IPA size using `Kvm::create_vm_with_ipa_size`.

### Removed
- Removed `Kvm::new_with_fd_number`. The same functionality is offered by the
  `Kvm` [FromRawFd](https://doc.rust-lang.org/std/os/unix/io/trait.FromRawFd.html)
  trait implementation.

### Changed
- The VM ioctl `unregister_ioevent` now correctly unregisters the events that
  correspond to the data match passed as a parameter.
- The `SystemEvent` Vcpu Exit now also contains the relevant type and flags.
- Updated `get_dirty_log` such that it does not assume the page size is 4K,
  but instead reads it using `libc::sysconf`.

## v0.5.0

### Added
- Support for the vcpu ioctls `KVM_GET/SET_VCPU_EVENTS` and `KVM_GET_DIRTY_LOG`
  on `aarch64`.
- Support for the vcpu ioctl `KVM_IRQ_LINE`.

## v0.4.0

### Added
- Support for unregistering ioeventfds through `KVM_IOEVENTFD`.

### Changed
- Functions working with event FDs now require
  vmm_sys_util::eventfd::EventFd in their interface instead of
  RawFd.
- Functions working with FAM structs kvm_msr_list and kvm_msrs, were
  changed to work with their respective safe counterparts MsrList and
  respectively Msrs.
- Now exporting kvm_ioctls::Error type definition so that users of this
  crate can create their own wrapping errors without having to know the
  Error type used internally by this crate.
- No longer exporting kvm_ioctls::Result. Users of this crate should
  not have to use kvm_ioctls::Result outside the crate.
- kvm_ioctls::Error now works with errno::Error instead of io::Error.

### Removed
- CpuId safe wrapper over FAM struct kvm_cpuid2. The safe wrapper is
  now provided by the kvm_bindings crate starting with v0.2.0.
- KVM_MAX_MSR_ENTRIES and MAX_KVM_CPUID_ENTRIES. Equivalent constants
  are provided by the kvm_bindings crate starting with v0.2.0.

## v0.3.0

### Added
- Support for setting vcpu `kvm_immediate_exit` flag
- Support for the vcpu ioctl `KVM_GET_CPUID2`
- Support for the vcpu ioctl `KVM_GET_MP_STATE`
- Support for the vcpu ioctl `KVM_SET_MP_STATE`
- Support for the vcpu ioctl `KVM_GET_VCPU_EVENTS`
- Support for the vcpu ioctl `KVM_SET_VCPU_EVENTS`
- Support for the vcpu ioctl `KVM_GET_DEBUGREGS`
- Support for the vcpu ioctl `KVM_SET_DEBUGREGS`
- Support for the vcpu ioctl `KVM_GET_XSAVE`
- Support for the vcpu ioctl `KVM_SET_XSAVE`
- Support for the vcpu ioctl `KVM_GET_XCRS`
- Support for the vcpu ioctl `KVM_SET_XCRS`
- Support for the vm ioctl `KVM_GET_IRQCHIP`
- Support for the vm ioctl `KVM_SET_IRQCHIP`
- Support for the vm ioctl `KVM_GET_CLOCK`
- Support for the vm ioctl `KVM_SET_CLOCK`
- Support for the vm ioctl `KVM_GET_PIT2`
- Support for the vm ioctl `KVM_SET_PIT2`
- Support for the vcpu ioctl `KVM_GET_ONE_REG`

### Changed
- Function offering support for `KVM_SET_MSRS` also returns the number
  of MSR entries successfully written.

## v0.2.0

### Added
- Add support for `KVM_ENABLE_CAP`.
- Add support for `KVM_SIGNAL_MSI`.

### Fixed
- Fix bug in KvmRunWrapper. The memory for kvm_run struct was not unmapped
  after the KvmRunWrapper object got out of scope.
- Return proper value when receiving the EOI KVM exit.
- Mark set_user_memory_region as unsafe.

## v0.1.0

First release of the kvm-ioctls crate.

The kvm-ioctls crate provides safe wrappers over the KVM API, a set of ioctls
used for creating and configuring Virtual Machines (VMs) on Linux.
The ioctls are accessible through four structures:
- Kvm - wrappers over system ioctls
- VmFd - wrappers over VM ioctls
- VcpuFd - wrappers over vCPU ioctls
- DeviceFd - wrappers over device ioctls

The kvm-ioctls can be used on x86_64 and aarch64. Right now the aarch64
support is considered experimental.
