:orphan:

.. _migration_4.0:

Migration guide to Zephyr v4.0.0 (Working Draft)
################################################

This document describes the changes required when migrating your application from Zephyr v3.7.0 to
Zephyr v4.0.0.

Any other changes (not directly related to migrating applications) can be found in
the :ref:`release notes<zephyr_4.0>`.

.. contents::
    :local:
    :depth: 2

Build System
************

Kernel
******

Boards
******

* :ref:`native_posix<native_posix>` has been deprecated in favour of
  :ref:`native_sim<native_sim>` (:github:`76898`).

Modules
*******

Mbed TLS
========

* The Kconfig options ``CONFIG_MBEDTLS_TLS_VERSION_1_0`` and ``CONFIG_MBEDTLS_TLS_VERSION_1_1``
  have been removed because Mbed TLS doesn't support TLS 1.0 and 1.1 anymore since v3.0. (:github:`76833`)

Trusted Firmware-M
==================

LVGL
====

Device Drivers and Devicetree
*****************************

* The ``compatible`` of the LiteX ethernet controller has been renamed from
  ``litex,eth0`` to :dtcompatible:`litex,liteeth`. (:github:`75433`)

* The ``compatible`` of the LiteX uart controller has been renamed from
  ``litex,uart0`` to :dtcompatible:`litex,uart`. (:github:`74522`)

* The devicetree bindings for the Microchip ``mcp23xxx`` series have been split up. Users of
  ``microchip,mcp230xx`` and ``microchip,mcp23sxx`` should change their devicetree ``compatible``
  values to the specific chip variant, e.g. :dtcompatible:`microchip,mcp23017`.
  The ``ngpios`` devicetree property has been removed, since it is implied by the model name.
  Chip variants with open-drain outputs (``mcp23x09``, ``mcp23x18``) now correctly reflect this in
  their driver API, users of these devices should ensure they pass appropriate values to
  :c:func:`gpio_pin_set`. (:github:`65797`)

Controller Area Network (CAN)
=============================

Display
=======

Enhanced Serial Peripheral Interface (eSPI)
===========================================

GNSS
====

Input
=====

* :c:macro:`INPUT_CALLBACK_DEFINE` has now an extra ``user_data`` void pointer
  argument that can be used to reference any user data structure. To restore
  the current behavior it can be set to ``NULL``. A ``void *user_data``
  argument has to be added to the callback function arguments.

* The :dtcompatible:`analog-axis` ``invert`` property has been renamed to
  ``invert-input`` (there's now an ``invert-output`` available as well).

Interrupt Controller
====================

LED Strip
=========

SDHC
====

* The NXP USDHC driver now assumes a card is present if no card detect method
  is configured, instead of using the peripheral's internal card detect signal
  to check for card presence. To use the internal card detect signal, the
  devicetree property ``detect-cd`` should be added to the USDHC node in use.

Sensors
=======

Serial
======

Regulator
=========

* Internal regulators present in nRF52/53 series can now be configured using
  devicetree. The Kconfig options :kconfig:option:`CONFIG_SOC_DCDC_NRF52X`,
  :kconfig:option:`CONFIG_SOC_DCDC_NRF52X_HV`,
  :kconfig:option:`CONFIG_SOC_DCDC_NRF53X_APP`,
  :kconfig:option:`CONFIG_SOC_DCDC_NRF53X_NET` and
  :kconfig:option:`CONFIG_SOC_DCDC_NRF53X_HV` selected by board-level Kconfig
  options have been deprecated.

  Example for nRF52 series:

  .. code-block:: devicetree

      /* configure REG/REG1 in DC/DC mode */
      &reg/reg1 {
          regulator-initial-mode = <NRF5X_REG_MODE_DCDC>;
      };

      /* enable REG0 (HV mode) */
      &reg0 {
          status = "okay";
      };

  Example for nRF53 series:

  .. code-block:: devicetree

      /* configure VREGMAIN in DC/DC mode */
      &vregmain {
          regulator-initial-mode = <NRF5X_REG_MODE_DCDC>;
      };

      /* configure VREGRADIO in DC/DC mode */
      &vregradio {
          regulator-initial-mode = <NRF5X_REG_MODE_DCDC>;
      };

      /* enable VREGH (HV mode) */
      &vregh {
          status = "okay";
      };

Bluetooth
*********

Bluetooth HCI
=============

Bluetooth Mesh
==============

Bluetooth Audio
===============

* The Volume Renderer callback functions :code:`bt_vcp_vol_rend_cb.state` and
  :code:`bt_vcp_vol_rend_cb.flags` for VCP now contain an additional parameter for
  the connection.
  This needs to be added to all instances of VCP Volume Renderer callback functions defined.
  (:github:`76992`)

Bluetooth Classic
=================

Bluetooth Host
==============

Bluetooth Crypto
================

Networking
**********

* The CoAP public API functions :c:func:`coap_get_block1_option` and
  :c:func:`coap_get_block2_option` have changed. The ``block_number`` pointer
  type has changed from ``uint8_t *`` to ``uint32_t *``. Additionally,
  :c:func:`coap_get_block2_option` now accepts an additional ``bool *has_more``
  parameter, to store the value of the more flag. (:github:`76052`)

* The Ethernet bridge shell is moved under network shell. This is done so that
  all the network shell activities can be found under ``net`` shell command.
  After this change the bridge shell is used by ``net bridge`` command.

Other Subsystems
****************

Flash map
=========

hawkBit
=======

MCUmgr
======

Modem
=====

Architectures
*************
