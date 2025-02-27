.. SPDX-FileCopyrightText: 2019-2020 Intel Corporation
..
.. SPDX-License-Identifier: CC-BY-4.0

.. _onemkl_vm_exp:

exp
===


.. container::


   Computes an exponential of vector elements.


   .. container:: section


      .. rubric:: Syntax
         :class: sectiontitle


      Buffer API:


      .. code-block:: cpp


            namespace oneapi::mkl::vm {

            sycl::event exp(
                    sycl::queue& exec_queue,
                    std::int64_t n,
                    sycl::buffer<T,1>& a,
                    sycl::buffer<T,1>& y,
                    oneapi::mkl::vm::mode mode = oneapi::mkl::vm::mode::not_defined,
                    oneapi::mkl::vm::error_handler<T> errhandler = {});

            } // namespace oneapi::mkl::vm



      USM API:


      .. code-block:: cpp


            namespace oneapi::mkl::vm {

            sycl::event exp(
                    sycl::queue& exec_queue,
                    std::int64_t n,
                    const T *a,
                    T* y,
                    sycl::vector_class<sycl::event> const & depends = {},
                    oneapi::mkl::vm::mode mode = oneapi::mkl::vm::mode::not_defined,
                    oneapi::mkl::vm::error_handler<T> errhandler = {});

            } // namespace oneapi::mkl::vm



      ``exp`` supports the following precisions.


      .. list-table::
         :header-rows: 1

         * - T
         * - ``float``
         * - ``double``
         * - ``std::complex<float>``
         * - ``std::complex<double>``




.. container:: section


   .. rubric:: Description
      :class: sectiontitle


   The exp(a) function computes an exponential of vector elements.


   .. container:: tablenoborder


      .. list-table::
         :header-rows: 1

         * - Data Type
           - Threshold Limitations on Input Parameters
         * - single precision
           - ``a[i] < Log( FLT_MAX )``
         * - double precision
           - ``a[i] < Log( DBL_MAX )``




   .. container:: tablenoborder


      .. list-table::
         :header-rows: 1

         * - Argument
           - Result
           - Status code
         * - +0
           - +1
           -  
         * - -0
           - +1
           -  
         * - a > overflow
           - +∞
           - ``oneapi::mkl::vm::status::overflow``
         * - a < underflow
           - +0
           - ``oneapi::mkl::vm::status::overflow``
         * - +∞
           - +∞
           -  
         * - -∞
           - +0
           -  
         * - QNAN
           - QNAN
           -  
         * - SNAN
           - QNAN
           -  




   .. container:: tablenoborder


      .. list-table::
         :header-rows: 1

         * -
           -
           -
           -
           -
           -
           -
           -
         * - +i·∞
           -
           -
           -
           -
           -
           -
           -
         * - +i·Y
           -
           -  
           -  
           -  
           -  
           -
           -
         * - +i·0
           -
           -  
           -
           -
           -  
           -
           -
         * - -i·0
           -
           -  
           -
           -
           -  
           -
           -
         * - -i·Y
           -
           -  
           -  
           -  
           -  
           -
           -
         * - -i·∞
           -
           -
           -
           -
           -
           -
           -
         * - +i·NAN
           -
           -
           -
           -
           -
           -
           -




   Notes:


   - The complex exp(z) function sets the VM status code to
      ``oneapi::mkl::vm::status::overflow`` in the case of overflow, 
      that is, when both ``RE(z)`` and ``IM(z)`` are finite non-zero
      numbers, but the real or imaginary part of the exact  result
      is so large that it does not meet the target precision.



.. container:: section


   .. rubric:: Input Parameters
      :class: sectiontitle


   Buffer API:


   exec_queue
      The queue where the routine should be executed.


   n
      Specifies the number of elements to be calculated.


   a
      The buffer ``a`` containing input vector of size ``n``.


   mode
      Overrides the global VM mode setting for this function call. See
      :ref:`onemkl_vm_setmode`
      function for possible values and their description. This is an
      optional parameter. The default value is ``oneapi::mkl::vm::mode::not_defined``.


   errhandler
      Sets local error handling mode for this function call. See the
      :ref:`onemkl_vm_create_error_handler`
      function for arguments and their descriptions. This is an optional
      parameter. The local error handler is disabled by default.


   USM API:


   exec_queue
      The queue where the routine should be executed.


   n
      Specifies the number of elements to be calculated.


   a
      Pointer ``a`` to the input vector of size ``n``.


   depends
      Vector of dependent events (to wait for input data to be ready).


   mode
      Overrides the global VM mode setting for this function call. See
      the :ref:`onemkl_vm_setmode`
      function for possible values and their description. This is an
      optional parameter. The default value is ``oneapi::mkl::vm::mode::not_defined``.


   errhandler
      Sets local error handling mode for this function call. See the
      :ref:`onemkl_vm_create_error_handler`
      function for arguments and their descriptions. This is an optional
      parameter. The local error handler is disabled by default.


.. container:: section


   .. rubric:: Output Parameters
      :class: sectiontitle


   Buffer API:


   y
      The buffer ``y`` containing the output vector of size ``n``.


   USM API:


   y
      Pointer ``y`` to the output vector of size ``n``.


   return value (event)
      Event, signifying availability of computed output and status code(s).

.. container:: section


    .. rubric:: Exceptions
        :class: sectiontitle

    For list of generated exceptions please refer to  :ref:`onemkl_vm_exceptions`


.. container:: familylinks


   .. container:: parentlink

      **Parent topic:** :ref:`onemkl_vm_mathematical_functions`


