.. SPDX-FileCopyrightText: 2021 Intel Corporation
..
.. SPDX-License-Identifier: CC-BY-4.0

=======================
collaborative_call_once
=======================
**[algorithms.collaborative_call_once]**

Function template that executes function exactly once.

.. code:: cpp

    // Defined in header <oneapi/tbb/collaborative_call_once.h>

    namespace oneapi {
        namespace tbb {

            template<typename Func, typename... Args>
            void collaborative_call_once(collaborative_once_flag& flag, Func&& func, Args&&... args);

        } // namespace tbb
    } // namespace oneapi

Requirements:

* ``Func`` type must meet the ``Function Objects``
  requirements from the [function.objects] section of the ISO C++ Standard section.

Executes the ``Func`` object only once, even if it is called concurrently. It allows other threads
blocked on the same ``collaborative_once_flag`` to join oneTBB parallel construction called
within the ``Func`` object.

In case of the exception thrown from the ``Func`` object, the thread calling the ``Func`` object 
receives this exception. One of the threads blocked on the same ``collaborative_once_flag``
calls the ``Func`` object again. 

collaborative_once_flag Class
-----------------------------

.. toctree::
    :titlesonly:

    collaborative_once_flag_cls.rst


Example
-------

The following example shows a class in which the "Lazy initialization" pattern is implemented on 
the "cachedProperty" field.


.. code:: cpp

    #include "oneapi/tbb/collaborative_call_once.h"

    extern double foo(int i);

    class LazyData {
        oneapi::tbb::collaborative_once_flag flag;
        double cachedProperty;
    public:
        double getProperty() {
            oneapi::tbb::collaborative_call_once(flag, [&] {
                // serial part
                double result{};

                // parallel part where threads can collaborate
                result = oneapi::tbb::parallel_reduce(oneapi::tbb::blocked_range<int>(0, 1000),
                    [&] (auto r, double r) {
                        for(int i = r.begin(); i != r.end(); ++i) {
                            r += foo(i);
                        }
                        return r;
                    },
                    std::plus<double, double>{}
                );

                // continue serial part
                cachedProperty = result;
            });

            return cachedProperty;
        }
    };
