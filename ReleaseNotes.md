# Release Notes

## 0.4.0-alpha.1

- Update README for 0.4.0

- update vboxj*.jar to version 4.3.6

- Use vmfest-0.4.0-alpha.1

- Update vmfest to 0.4.0

## 0.3.0-RC.1

- Use vmfest 0.3.0

- Move protocols to their own namespace

## 0.3.0-beta.2

- Update to vmfest 0.3.0-beta.3

- Honor :model-path when installing image.

## 0.3.0-beta.1

- Update to vmfest 0.3.0-beta.2.

## 0.3.0-alpha.6

- Update to vmfest 0.3.0-beta.1.

- Default to io-apic-enabled? set to true for all VMs.

- Fix forwarding of options for vmfest/add-image and vmfest/install-image.

- Fix vmfest-script task to correctly instantiate the vmfest provider.

- Allow specification of hardware in image meta
  In order for an image to be able to specify hardware requirements, use the
  meta :hardware key in determining the machine model.

- Use own image => use vmfest-image; simplify instructions; link to vmfest
  project

- Honor `:node-type`, `:bridged-interface` and `:local-interface` in
  `node-spec`
  Fixes #10.

  These entries go under `:hardware` in the node-spec. These entries will
  override any provider-specific network settings and the defaults values
  when none is specified.

  With this fix, pallet now will honor these directives. You can pass
  `:node-type` (`:bridged` or `:local`) to the node spec, and also which
  network interface to use in each case via `:bridged-interface` and
  `:local-interface`, e.g:

  ```clojure
  (pallet.api/node-spec :image {:image-id :ubuntu-12.04}
                       :hardware {:network-type :bridged
                                  :bridged-interface "en0: Ethernet"}))
  ```


## 0.3.0-alpha.5

- Update vmfest to 0.3.0-alpha.5.

- Fix issue with forwards from p.c.vmfest to p.c.v.service.

## 0.3.0-alpha.4

- Remove use of slingshot

- Ignore stopped/inaccessible VMs

  If a VM is misconfigured, or stopped, pallet cannot manage it. From now on
  these VMs will be effectively ignored. For example, if you have a
  `vm-test-0` VM that is stopped, and you converge to 1 the same group, you
  will get another VM in this group. Converging back to 0 will remove this
  new vm, leaving the stopped `vm-test-0` alone.

  Similarly, all VMs created by pallet are in the `vmfest` group in
  VirtualBox, and pallet will not _see_ any VM outside of this group. The
  VMs outside the pallet group were already ignored, since they don't have
  any group tag, but now they will not even be considered (e.g. not listed
  in pallet.repl/show-nodes).

- Allow dynamic selection of the method of comm w/vbox

  The vmfest provider now can cake a new parameter `:vbox-comm` to select
  the method of communication of Pallet with the VirtualBox subsystem. The
  options are :xpcom for XPCOM, and :ws for Web Services. It defaults to
  :xpcom.

  There is no longer the need to add either vboxjxws or vboxjxpcom jars to
  the classpath prior to using pallet-vmfest, in order to select the
  communication method. Pallet-vmfest contains both jars and one of them
  will be injected in the classpath dynamically depending on the value of
  `:vbox-comm`.

  NOTE: Selecting the communication methods with VirtualBox can only be done
  once per JVM session.

- Use lein instead of maven, deploy to clojars

  The group-id is now com.palletops.

## 0.3.0-alpha.3

- Update to vmfest 0.3.0-alpha.3

- Fix image install from 'lein pallet' where .meta was ignored.
  This fix adds the requirement to always specify the 'os-64-bit' when
  installing Vagrant boxes.

- Remove vbox objects from hardware map
  When using xpcom, the :wrapped and :typedWrapped fields were showing up.

## 0.3.0-alpha.2

- Correct vbox.home processing when property set
  An existing value for the property was being treated as a file.  Also
  adds logging of the value used and it's provenance.

## 0.3.0-alpha.1

- Update to vmfest 0.3.0-alpha.2

- Update add-vmfest-image task to take options
  Allows specification of os-family, os-version and os-64-bit for import of
  vagrant .box images.

- Set up nat rules for ssh forwarding when using nat

- Propogate :nat-rules from meta files

- Ensure image-id is converted to a keyword

- Add NodeProxy protocol support

- Add xpcom profile to pom

- Try and guess the vbox.home directory

## 0.2.4

- Update to vmfest 0.2.9
  Removes a logback.xml configuration file.

## 0.2.3

- Update to vmfest 0.2.8
  This is to import the fix that ensures the models are immutable before
  using them.

## 0.2.2

- Add ComputeServiceProperties implementation

- Adjust logging

- Update to vmfest 0.2.7 which supports VirtualBox 4.2.x.

- Ignore inaccessible Machines
  VirtualBox Machines that are not accessible can't be inspected. Pallet
  assumes it can query all the nodes, but this breaks if the node (machine)
  is not accessible.

  - Make pallet.compute/nodes return accessible nodes only.
  - unify the way in which machine accessibility is assessed.


## 0.2.1

- Implement NodeHardware protocol

- Update to vmfest 0.2.5

- Add support for 0.8.0-SNAPSHOT post alpha.1


## 0.2.0

- Use the released version of vmfest 0.2.4.

- Disable pty for fedora
  Fedora seems to flush standard input when using a pty, and so
  intermittently misses the pallet scripts being sent to it.

- Allow override of destruction of vm on IP acquistion failure

- Allow image metadata to overide hardware model storage
  Images boot on either SATA or IDE but not both, so the storage device
  required on the hardware is image dependent.

- Allow specification of the :network-type on the image metadata
  Allows the image to specify it's preferred network connection type

## 0.2.0-beta.3

- Update to vmfest 0.2.4-beta.4

- Update to latest parent-pom

- Fix for protocol implementations that call each other
  This was broken by the wrapping of the nodes in a deftype

- Enable specification of packager in vmfest metadata

- Detect if the pallet version supports multi-lang scripts

- Add an add-vmfest-image task

- Remove debug print forms

## 0.2.0-beta.2

- Update vmfest dependency to 0.2.4-beta.3

- Allow selecting a vmfest image via :image-id. Fixes #2

- Wait for all network interfaces in a VM have an IP address before
  proceeding. Fixes #3

- Allow override of image destroy on error with :destroy-on-bootstrap-fail

- Fixes #1 : The host-only interface was not forwarded correctly to vmfest
  when using :local networking.

## 0.2.0-beta.1

Initial version.
