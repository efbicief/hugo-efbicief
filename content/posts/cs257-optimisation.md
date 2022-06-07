---
title: "Code Optimisation - Parallelisation"
date: 2022-06-06T10:22:41+01:00
draft: false
tags: [ "CS257", "Architecture", "Optimisation", "Notes" ]
---
For this section of the notes, we will work through the following past question from the 2020 paper, using SIMD instructions.

> - The simulate function below is taken from a compute intensive simulation program written in C.
{{< highlight c >}}
  void simulate() {
    //Loop 1
    for (int i = 0; i < N; i++) {
      ax[i] = 0.0f;
      ay[i] = 0.0f;
      az[i] = 0.0f;
    }

    //Loop 2
    for (int i = 0; i < N; i++) {
      for (int j = 0; j < N; j++) {
        float rx = x[j] - x[i];
        float ry = y[j] - y[i];
        float rz = z[j] - z[i];
        float r2 = rx*rx + ry*ry + rz*rz + eps;
        float r2inv = 1.0f / sqrt(r2);
        float r6inv = r2inv * r2inv * r2inv;
        float s = m[j] * r6inv;
        ax[i] += s * rx;
        ay[i] += s * ry;
        az[i] += s * rz;
      }
    }

    //Loop 3
    for (int i = 0; i < N; i++) {
      vx[i] += dmp * (dt * ax[i]);
      vy[i] += dmp * (dt * ay[i]);
      vz[i] += dmp * (dt * az[i]);
    }

    //Loop 4
    for (int i = 0; i < N; i++) {
      x[i] += dt * vx[i];
      y[i] += dt * vy[i];
      z[i] += dt * vz[i];
      if (x[i] >= 1.0f || x[i] <= -1.0f) vx[i] *= -1.0f;
      if (y[i] >= 1.0f || y[i] <= -1.0f) vy[i] *= -1.0f;
      if (z[i] >= 1.0f || z[i] <= -1.0f) vz[i] *= -1.0f;
    }

  }
{{< / highlight >}}
> - You may assume that the variables x,y,x,ax,ay,az,vx,vy,and vz are arrays of floats of length N that are aligned in memory. These arrays are not modified in any place other than the simulate function.
> - In answering this question you are expected to optimise the code by hand (this means that no #pragma functions or compiler options should be used) to improve single core performance.
> - You may assume that the target platform is a modern Intel processor, which has all MMX, SSE and AVX instructions available.
> - You might find it helpful to add comments to you code to explain your intentions.

- Optimise the loop identified as Loop 1 in the simulate function.

{{< highlight c >}}
  // Before
  for (int i = 0; i < N; i++) {
    ax[i] = 0.0f;
    ay[i] = 0.0f;
    az[i] = 0.0f;
  }

  // After
  // Using 128-bit vector units, which can hold 4 32-bit floats

  #include <immintrin.h>
  int loopN = (N/4)*4; // Number of items that can be vectorised

  for (int i = 0; i < loopN; i += 4) {
    // Write contents of zero_vec to ax, ay and az + offset i
    _mm_store_ps(ax+i, _mm_setzero_ps());
    _mm_store_ps(ay+i, _mm_setzero_ps());
    _mm_store_ps(ax+i, _mm_setzero_ps());
  }

  for (; i < N; i++) {
    ax[i] = 0.0f;
    ay[i] = 0.0f;
    az[i] = 0.0f;
  }
{{< / highlight >}}

- Optimise the loop identified as Loop 2 in the simulate function.

{{< highlight c >}}
  // Before
  for (int i = 0; i < N; i++) {
    for (int j = 0; j < N; j++) {
      float rx = x[j] - x[i];
      float ry = y[j] - y[i];
      float rz = z[j] - z[i];
      float r2 = rx*rx + ry*ry + rz*rz + eps;
      float r2inv = 1.0f / sqrt(r2);
      float r6inv = r2inv * r2inv * r2inv;
      float s = m[j] * r6inv;
      ax[i] += s * rx;
      ay[i] += s * ry;
      az[i] += s * rz;
    }
  }

  // After

  #include <immintrin.h>
  int loopN = (N/4)*4;
  __m128 eps_vec = _mm_set1_ps(eps);

  for (int i = 0; i < loopN; i+=4) {
    // Loading needed vectors (i-indexed)
    __m128 ax_vec = _mm_load_ps(ax+i);
    __m128 ay_vec = _mm_load_ps(ay+i);
    __m128 az_vec = _mm_load_ps(az+i);
    __m128 xi_vec = _mm_load_ps(x+i);
    __m128 yi_vec = _mm_load_ps(y+i);
    __m128 zi_vec = _mm_load_ps(z+i);
    for (int j = 0; j < loopN; j+=4) {
      // Loading needed vectors (j-indexed)
      __m128 xj_vec = _mm_load_ps(x+j);
      __m128 yj_vec = _mm_load_ps(y+j);
      __m128 zj_vec = _mm_load_ps(z+j);
      __m128 m_vec = _mm_load_ps(m+j);
      // Computation
      __m128 rx = _mm_sub_ps(xj, xi);
      __m128 ry = _mm_sub_ps(yj, yi);
      __m128 rz = _mm_sub_ps(zj, zi);
      __m128 r2 = _mm_add_ps(_mm_add_ps(_mm_mul_ps(rx, rx), _mm_mul_ps(ry, ry)), _mm_add_ps(_mm_mul_ps(rz, rz), eps_vec));
      __m128 r2inv = _mm_invsqrt(r2);
      __m128 r6inv = _mm_mul_ps(_mm_mul_ps(r2inv, r2inv), r2inv);
      __m128 s = _mm_mul_ps(m_vec, r6inv);
      ax_vec = _mm_add_ps(ax_vec, _mm_mul_ps(s, rx));
      ay_vec = _mm_add_ps(ay_vec, _mm_mul_ps(s, ry));
      az_vec = _mm_add_ps(az_vec, _mm_mul_ps(s, rz));
      // Storing back into arrays
      _mm_store_ps(ax+i, ax_vec);
      _mm_store_ps(ay+i, ay_vec);
      _mm_store_ps(az+i, az_vec);
    }
    for (; j < N; j++) {
      float rx = x[j] - x[i];
      float ry = y[j] - y[i];
      float rz = z[j] - z[i];
      float r2 = rx*rx + ry*ry + rz*rz + eps;
      float r2inv = 1.0f / sqrt(r2);
      float r6inv = r2inv * r2inv * r2inv;
      float s = m[j] * r6inv;
      ax[i] += s * rx;
      ay[i] += s * ry;
      az[i] += s * rz;
    }
  }

  // Cleanup
  for (; i < N; i++) {
    for (int j = 0; j < loopN; j+=4) {
      // Loading needed vectors (j-indexed)
      __m128 xj_vec = _mm_load_ps(x+j);
      __m128 yj_vec = _mm_load_ps(y+j);
      __m128 zj_vec = _mm_load_ps(z+j);
      __m128 m_vec = _mm_load_ps(m+j);
      // Computation
      __m128 rx = _mm_sub_ps(xj, xi);
      __m128 ry = _mm_sub_ps(yj, yi);
      __m128 rz = _mm_sub_ps(zj, zi);
      __m128 r2 = _mm_add_ps(_mm_add_ps(_mm_mul_ps(rx, rx), _mm_mul_ps(ry, ry)), _mm_add_ps(_mm_mul_ps(rz, rz), eps_vec));
      __m128 r2inv = _mm_invsqrt(r2);
      __m128 r6inv = _mm_mul_ps(_mm_mul_ps(r2inv, r2inv), r2inv);
      __m128 s = _mm_mul_ps(m_vec, r6inv);
      ax_vec = _mm_add_ps(ax_vec, _mm_mul_ps(s, rx));
      ay_vec = _mm_add_ps(ay_vec, _mm_mul_ps(s, ry));
      az_vec = _mm_add_ps(az_vec, _mm_mul_ps(s, rz));
      // Storing back into arrays
      _mm_store_ps(ax+i, ax_vec);
      _mm_store_ps(ay+i, ay_vec);
      _mm_store_ps(az+i, az_vec);
    }
    for (; j < N; j++) {
      float rx = x[j] - x[i];
      float ry = y[j] - y[i];
      float rz = z[j] - z[i];
      float r2 = rx*rx + ry*ry + rz*rz + eps;
      float r2inv = 1.0f / sqrt(r2);
      float r6inv = r2inv * r2inv * r2inv;
      float s = m[j] * r6inv;
      ax[i] += s * rx;
      ay[i] += s * ry;
      az[i] += s * rz;
    }
  }
  
{{< / highlight >}}

- Optimise the loop identified as Loop 3 in the simulate function.

{{< highlight c >}}
  // Before
  for (int i = 0; i < N; i++) {
    vx[i] += dmp * (dt * ax[i]);
    vy[i] += dmp * (dt * ay[i]);
    vz[i] += dmp * (dt * az[i]);
  }

  // After

  #include <immintrin.h>
  int loopN = (N/4)*4;
  __m128 dmp_vec = _mm_set1_ps(dmp);
  __m128 dt_vec = _mm_set1_ps(dt);

  for (int i = 0; i < N; i++) {
    // Load vectors
    __m128 vx_vec = _mm_load_ps(vx+i);
    __m128 vy_vec = _mm_load_ps(vy+i);
    __m128 vz_vec = _mm_load_ps(vx+i);
    __m128 ax_vec = _mm_load_ps(ax+i);
    __m128 ay_vec = _mm_load_ps(ay+i);
    __m128 az_vec = _mm_load_ps(az+i);
    // Calculate
    vx_vec = _mm_add_ps(vx_vec, _mm_mul_ps(dmp_vec, _mm_mul_ps(dt_vec, ax_vec)));
    vy_vec = _mm_add_ps(vy_vec, _mm_mul_ps(dmp_vec, _mm_mul_ps(dt_vec, ay_vec)));
    vz_vec = _mm_add_ps(vz_vec, _mm_mul_ps(dmp_vec, _mm_mul_ps(dt_vec, az_vec)));
    // Store
    _mm_store_ps(vx+i, vx_vec);
    _mm_store_ps(vy+i, vy_vec);
    _mm_store_ps(vz+i, vz_vec);
  }
{{< / highlight >}}

- Optimise the loop identified as Loop 4 in the simulate function.

{{< highlight c >}}
  // Before
  for (int i = 0; i < N; i++) {
      x[i] += dt * vx[i];
      y[i] += dt * vy[i];
      z[i] += dt * vz[i];
      if (x[i] >= 1.0f || x[i] <= -1.0f) vx[i] *= -1.0f;
      if (y[i] >= 1.0f || y[i] <= -1.0f) vy[i] *= -1.0f;
      if (z[i] >= 1.0f || z[i] <= -1.0f) vz[i] *= -1.0f;
    }

  // After

  #include <immintrin.h>
  int loopN = (N/4)*4;
  __m128 dt_vec = _mm_set1_ps(dt);
  __m128 onevec = _mm_set1_ps(1.0f);
  __m128 negonevec = _m_set1_ps(-1.0f);

  for (int i = 0; i < N; i++) {
    // Load vectors
    __m128 x_vec = _mm_load_ps(x+i);
    __m128 y_vec = _mm_load_ps(y+i);
    __m128 z_vec = _mm_load_ps(z+i);
    __m128 vx_vec = _mm_load_ps(vx+i);
    __m128 vy_vec = _mm_load_ps(vy+i);
    __m128 vz_vec = _mm_load_ps(vz+i);
    // Compute
    x_vec = _mm_add_ps(x_vec, _mm_mul_ps(dt_vec, vx_vec));
    y_vec = _mm_add_ps(y_vec, _mm_mul_ps(dt_vec, vy_vec));
    z_vec = _mm_add_ps(z_vec, _mm_mul_ps(dt_vec, vz_vec));
    
    __m128 mask_x = _mm_and_ps(_mm_cmpge_ps(x_vec, onevec), _mm_cmple_ps(x_vec, negonevec));
    __m128 branch1vx = _mm_mul_ps(vx_vec, negonevec);
    __m128 vx_vec = _mm_blendv_ps(vx_vec, branch1vx, mask_x);

    __m128 mask_y = _mm_and_ps(_mm_cmpge_ps(y_vec, onevec), _mm_cmple_ps(y_vec, negonevec));
    __m128 branch1vy = _mm_mul_ps(vy_vec, negonevec);
    __m128 vy_vec = _mm_blendv_ps(vy_vec, branch1vy, mask_y);

    __m128 mask_z = _mm_and_ps(_mm_cmpge_ps(z_vec, onevec), _mm_cmple_ps(z_vec, negonevec));
    __m128 branch1vx = _mm_mul_ps(vz_vec, negonevec);
    __m128 vz_vec = _mm_blendv_ps(vz_vec, branch1vz, mask_z);

    // Store
    _mm_store_ps(vx+i, vx_vec);
    _mm_store_ps(vy+i, vy_vec);
    _mm_store_ps(vz+i, vz_vec);
  }
{{< / highlight >}}

[All topics ⟶](/posts/cs257-index/)
