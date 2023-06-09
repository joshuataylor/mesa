Rusticl
=======

To build Rusticl you need to satisfy the following build dependencies:

-  ``rustc``
-  ``rustfmt`` (highly recommended, but only *required* for CI builds
   or when authoring patches)
-  ``bindgen``
-  `LLVM <https://github.com/llvm/llvm-project/>`__ built with
   ``libclc`` and ``-DLLVM_ENABLE_DUMP=ON``.
-  `SPIRV-Tools <https://github.com/KhronosGroup/SPIRV-Tools>`__
-  `SPIRV-LLVM-Translator
   <https://github.com/KhronosGroup/SPIRV-LLVM-Translator>`__ for a
   ``libLLVMSPIRVLib.so`` matching your version of LLVM, i.e. if you're
   using LLVM 15 (``libLLVM.so.15``), then you need a
   ``libLLVMSPIRVLib.so.15``.

The minimum versions to build Rusticl are:

-  Rust: 1.59
-  Meson: 1.0.0
-  Bindgen: 0.58.0
-  LLVM: 11.0.0 (recommended: 15.0.0)
-  SPIRV-Tools: any version (recommended: v2022.3)

Afterwards you only need to add ``-Dgallium-rusticl=true -Dllvm=enabled
-Drust_std=2021`` to your build options.

Most of the code related to Mesa's C code lives inside ``/mesa``, with
the occasional use of enums, structs or constants through the code base.

If you need help ping ``karolherbst`` either in ``#dri-devel`` or
``#rusticl`` on OFTC.

Contributing 
------------

The minimum configuration you need to start developing with rust
is ``RUSTC=clippy-driver meson configure -Dgallium-rusticl=true
-Dllvm=enabled -Drust_std=2021``. In addition you probably want to enable
any device drivers on your platform. Some device drivers as well as some
features are locked behind flags during runtime. See
:ref:`Rusticl environment variables <rusticl-env-var>` for
more info.

All patches that are potentially conformance breaking and also patches
that add new features should be ran against the appropriate conformance
tests.

Also, make sure the formatting is in order before submitting code. That
can easily be done via ``git ls-files */{lib,app}.rs | xargs rustfmt``.

When submitting Merge Requests or filing bugs related to Rusticl, make
sure to add the ``Rusticl`` label so people subscribed to that Label get
pinged.

Known issues
------------

One issue you might come across is, that the Rust edition meson sets is
not right. This is a known `meson bug
<https://github.com/mesonbuild/meson/issues/10664>`__ and in order to
fix it, simply run ``meson configure $your_build_dir -Drust_std=2021``
