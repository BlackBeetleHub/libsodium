cmake_minimum_required(VERSION 3.8)

project(libsodium)

set(VERSION "1.0.3")
set(SODIUM_LIBRARY_VERSION_MAJOR 7)
set(SODIUM_LIBRARY_VERSION_MINOR 5)

if("${CMAKE_CXX_COMPILER_ID}" MATCHES "MSVC")
    set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} /MP /GS /GL /analyze- /W3 /wd4146 /wd4244 /Gy /Gd /Zc:wchar_t /Zc:inline /fp:precise")
    set(CMAKE_STATIC_LINKER_FLAGS_RELEASE "${CMAKE_STATIC_LINKER_FLAGS_RELEASE} /LTCG /MACHINE:X64 /NOLOGO")
    add_definitions(
            -DSODIUM_STATIC
            -D_CONSOLE -Dinline=__inline
            -DNATIVE_LITTLE_ENDIAN
            -D_CRT_SECURE_NO_WARNINGS
            -D_LIB -DNDEBUG -DUNICODE
            -D_UNICODE -DWIN32
            -D_WIN32 -DWIN64 -D_WIN64)
endif()

if((${CMAKE_COMPILER_IS_GNUCXX}) OR (${CMAKE_C_COMPILER_ID} MATCHES "Clang"))
    message(INFO " I'm clang or gnu")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g -O2 -fvisibility=hidden -fPIC -fPIE -fno-strict-aliasing -fno-strict-overflow -fstack-protector -Winit-self -Wwrite-strings -Wdiv-by-zero")
    add_definitions(
            -DPACKAGE_NAME=\"libsodium\"
            -DPACKAGE_TARNAME=\"libsodium\"
            -DPACKAGE_VERSION=\"1.0.3\"
            -DPACKAGE_STRING=\"libsodium\ 1.0.3\"
            -DPACKAGE_BUGREPORT=\"https://github.com/jedisct1/libsodium/issues\"
            -DPACKAGE_URL=\"https://github.com/jedisct1/libsodium\"
            -DPACKAGE=\"libsodium\" -DVERSION=\"1.0.3\"
            -DSTDC_HEADERS=1
            -DHAVE_SYS_TYPES_H=1
            -DHAVE_SYS_STAT_H=1
            -DHAVE_STDLIB_H=1
            -DHAVE_STRING_H=1
            -DHAVE_MEMORY_H=1
            -DHAVE_STRINGS_H=1
            -DHAVE_INTTYPES_H=1
            -DHAVE_STDINT_H=1
            -DHAVE_UNISTD_H=1
            -D__EXTENSIONS__=1
            -D_ALL_SOURCE=1
            -D_GNU_SOURCE=1
            -D_POSIX_PTHREAD_SEMANTICS=1
            -D_TANDEM_SOURCE=1
            -DHAVE_DLFCN_H=1
            -DLT_OBJDIR=\".libs/\"
            -DHAVE_MMINTRIN_H=1
            -DHAVE_EMMINTRIN_H=1
            -DHAVE_PMMINTRIN_H=1
            -DHAVE_TMMINTRIN_H=1
            -DHAVE_SYS_MMAN_H=1
            -DNATIVE_LITTLE_ENDIAN=1
            -DHAVE_AMD64_ASM=1
            -DHAVE_TI_MODE=1
            -DHAVE_CPUID=1
            -DHAVE_WEAK_SYMBOLS=1
            -DHAVE_MMAP=1
            -DHAVE_MLOCK=1
            -DHAVE_MADVISE=1
            -DHAVE_MPROTECT=1
            -DHAVE_POSIX_MEMALIGN=1)
endif()

configure_file(src/libsodium/include/sodium/version.h.in ${CMAKE_CURRENT_SOURCE_DIR}/src/libsodium/include/sodium/version.h @ONLY)

include_directories(src/libsodium/include)
include_directories(src/libsodium/include/sodium)
include_directories(test/quirks)

set(ADD_SOURCE_SODIUM
        src/libsodium/crypto_aead/chacha20poly1305/sodium/aead_chacha20poly1305.c
        src/libsodium/crypto_pwhash/scryptsalsa208sha256/nosse/pwhash_scryptsalsa208sha256_nosse.c
        src/libsodium/crypto_pwhash/scryptsalsa208sha256/pwhash_scryptsalsa208sha256.c
        src/libsodium/crypto_pwhash/scryptsalsa208sha256/sse/pwhash_scryptsalsa208sha256_sse.c
        src/libsodium/crypto_scalarmult/curve25519/donna_c64/base_curve25519_donna_c64.c
        src/libsodium/crypto_scalarmult/curve25519/donna_c64/smult_curve25519_donna_c64.c
        src/libsodium/crypto_scalarmult/curve25519/ref10/base_curve25519_ref10.c
        src/libsodium/crypto_scalarmult/curve25519/ref10/fe_0_curve25519_ref10.c
        src/libsodium/crypto_scalarmult/curve25519/ref10/fe_1_curve25519_ref10.c
        src/libsodium/crypto_scalarmult/curve25519/ref10/fe_add_curve25519_ref10.c
        src/libsodium/crypto_scalarmult/curve25519/ref10/fe_copy_curve25519_ref10.c
        src/libsodium/crypto_scalarmult/curve25519/ref10/fe_cswap_curve25519_ref10.c
        src/libsodium/crypto_scalarmult/curve25519/ref10/fe_frombytes_curve25519_ref10.c
        src/libsodium/crypto_scalarmult/curve25519/ref10/fe_invert_curve25519_ref10.c
        src/libsodium/crypto_scalarmult/curve25519/ref10/fe_mul121666_curve25519_ref10.c
        src/libsodium/crypto_scalarmult/curve25519/ref10/fe_mul_curve25519_ref10.c
        src/libsodium/crypto_scalarmult/curve25519/ref10/fe_sq_curve25519_ref10.c
        src/libsodium/crypto_scalarmult/curve25519/ref10/fe_sub_curve25519_ref10.c
        src/libsodium/crypto_scalarmult/curve25519/ref10/fe_tobytes_curve25519_ref10.c
        src/libsodium/crypto_scalarmult/curve25519/ref10/scalarmult_curve25519_ref10.c
        src/libsodium/crypto_stream/chacha20/ref/stream_chacha20_ref.c
        src/libsodium/crypto_stream/chacha20/stream_chacha20_api.c
        src/libsodium/crypto_stream/salsa20/ref/stream_salsa20_ref.c
        src/libsodium/crypto_stream/salsa20/ref/xor_salsa20_ref.c
        src/libsodium/crypto_auth/crypto_auth.c
        src/libsodium/crypto_auth/hmacsha256/auth_hmacsha256_api.c
        src/libsodium/crypto_auth/hmacsha256/cp/hmac_hmacsha256.c
        src/libsodium/crypto_auth/hmacsha256/cp/verify_hmacsha256.c
        src/libsodium/crypto_auth/hmacsha512/auth_hmacsha512_api.c
        src/libsodium/crypto_auth/hmacsha512/cp/hmac_hmacsha512.c
        src/libsodium/crypto_auth/hmacsha512/cp/verify_hmacsha512.c
        src/libsodium/crypto_auth/hmacsha512256/auth_hmacsha512256_api.c
        src/libsodium/crypto_auth/hmacsha512256/cp/hmac_hmacsha512256.c
        src/libsodium/crypto_auth/hmacsha512256/cp/verify_hmacsha512256.c
        src/libsodium/crypto_box/crypto_box.c
        src/libsodium/crypto_box/crypto_box_easy.c
        src/libsodium/crypto_box/crypto_box_seal.c
        src/libsodium/crypto_box/curve25519xsalsa20poly1305/box_curve25519xsalsa20poly1305_api.c
        src/libsodium/crypto_box/curve25519xsalsa20poly1305/ref/after_curve25519xsalsa20poly1305.c
        src/libsodium/crypto_box/curve25519xsalsa20poly1305/ref/before_curve25519xsalsa20poly1305.c
        src/libsodium/crypto_box/curve25519xsalsa20poly1305/ref/box_curve25519xsalsa20poly1305.c
        src/libsodium/crypto_box/curve25519xsalsa20poly1305/ref/keypair_curve25519xsalsa20poly1305.c
        src/libsodium/crypto_core/hsalsa20/ref2/core_hsalsa20.c
        src/libsodium/crypto_core/hsalsa20/core_hsalsa20_api.c
        src/libsodium/crypto_core/salsa20/ref/core_salsa20.c
        src/libsodium/crypto_core/salsa20/core_salsa20_api.c
        src/libsodium/crypto_core/salsa2012/ref/core_salsa2012.c
        src/libsodium/crypto_core/salsa2012/core_salsa2012_api.c
        src/libsodium/crypto_core/salsa208/ref/core_salsa208.c
        src/libsodium/crypto_core/salsa208/core_salsa208_api.c
        src/libsodium/crypto_generichash/crypto_generichash.c
        src/libsodium/crypto_generichash/blake2/generichash_blake2_api.c
        src/libsodium/crypto_generichash/blake2/ref/blake2b-ref.c
        src/libsodium/crypto_generichash/blake2/ref/generichash_blake2b.c
        src/libsodium/crypto_hash/crypto_hash.c
        src/libsodium/crypto_hash/sha256/hash_sha256_api.c
        src/libsodium/crypto_hash/sha256/cp/hash_sha256.c
        src/libsodium/crypto_hash/sha512/hash_sha512_api.c
        src/libsodium/crypto_hash/sha512/cp/hash_sha512.c
        src/libsodium/crypto_onetimeauth/crypto_onetimeauth.c
        src/libsodium/crypto_onetimeauth/poly1305/onetimeauth_poly1305.c
        src/libsodium/crypto_onetimeauth/poly1305/onetimeauth_poly1305_api.c
        src/libsodium/crypto_onetimeauth/poly1305/onetimeauth_poly1305_try.c
        src/libsodium/crypto_onetimeauth/poly1305/donna/auth_poly1305_donna.c
        src/libsodium/crypto_onetimeauth/poly1305/donna/verify_poly1305_donna.c
        src/libsodium/crypto_pwhash/scryptsalsa208sha256/crypto_scrypt-common.c
        src/libsodium/crypto_pwhash/scryptsalsa208sha256/scrypt_platform.c
        src/libsodium/crypto_pwhash/scryptsalsa208sha256/pbkdf2-sha256.c
        src/libsodium/crypto_scalarmult/crypto_scalarmult.c
        src/libsodium/crypto_scalarmult/curve25519/scalarmult_curve25519_api.c
        src/libsodium/crypto_secretbox/crypto_secretbox.c
        src/libsodium/crypto_secretbox/crypto_secretbox_easy.c
        src/libsodium/crypto_secretbox/xsalsa20poly1305/secretbox_xsalsa20poly1305_api.c
        src/libsodium/crypto_secretbox/xsalsa20poly1305/ref/box_xsalsa20poly1305.c
        src/libsodium/crypto_shorthash/crypto_shorthash.c
        src/libsodium/crypto_shorthash/siphash24/shorthash_siphash24_api.c
        src/libsodium/crypto_shorthash/siphash24/ref/shorthash_siphash24.c
        src/libsodium/crypto_sign/crypto_sign.c
        src/libsodium/crypto_sign/ed25519/sign_ed25519_api.c
        src/libsodium/crypto_sign/ed25519/ref10/fe_0.c
        src/libsodium/crypto_sign/ed25519/ref10/fe_1.c
        src/libsodium/crypto_sign/ed25519/ref10/fe_add.c
        src/libsodium/crypto_sign/ed25519/ref10/fe_cmov.c
        src/libsodium/crypto_sign/ed25519/ref10/fe_copy.c
        src/libsodium/crypto_sign/ed25519/ref10/fe_frombytes.c
        src/libsodium/crypto_sign/ed25519/ref10/fe_invert.c
        src/libsodium/crypto_sign/ed25519/ref10/fe_isnegative.c
        src/libsodium/crypto_sign/ed25519/ref10/fe_isnonzero.c
        src/libsodium/crypto_sign/ed25519/ref10/fe_mul.c
        src/libsodium/crypto_sign/ed25519/ref10/fe_neg.c
        src/libsodium/crypto_sign/ed25519/ref10/fe_pow22523.c
        src/libsodium/crypto_sign/ed25519/ref10/fe_sq.c
        src/libsodium/crypto_sign/ed25519/ref10/fe_sq2.c
        src/libsodium/crypto_sign/ed25519/ref10/fe_sub.c
        src/libsodium/crypto_sign/ed25519/ref10/fe_tobytes.c
        src/libsodium/crypto_sign/ed25519/ref10/ge_add.c
        src/libsodium/crypto_sign/ed25519/ref10/ge_double_scalarmult.c
        src/libsodium/crypto_sign/ed25519/ref10/ge_frombytes.c
        src/libsodium/crypto_sign/ed25519/ref10/ge_madd.c
        src/libsodium/crypto_sign/ed25519/ref10/ge_msub.c
        src/libsodium/crypto_sign/ed25519/ref10/ge_p1p1_to_p2.c
        src/libsodium/crypto_sign/ed25519/ref10/ge_p1p1_to_p3.c
        src/libsodium/crypto_sign/ed25519/ref10/ge_p2_0.c
        src/libsodium/crypto_sign/ed25519/ref10/ge_p2_dbl.c
        src/libsodium/crypto_sign/ed25519/ref10/ge_p3_0.c
        src/libsodium/crypto_sign/ed25519/ref10/ge_p3_dbl.c
        src/libsodium/crypto_sign/ed25519/ref10/ge_p3_to_cached.c
        src/libsodium/crypto_sign/ed25519/ref10/ge_p3_to_p2.c
        src/libsodium/crypto_sign/ed25519/ref10/ge_p3_tobytes.c
        src/libsodium/crypto_sign/ed25519/ref10/ge_precomp_0.c
        src/libsodium/crypto_sign/ed25519/ref10/ge_scalarmult_base.c
        src/libsodium/crypto_sign/ed25519/ref10/ge_sub.c
        src/libsodium/crypto_sign/ed25519/ref10/ge_tobytes.c
        src/libsodium/crypto_sign/ed25519/ref10/keypair.c
        src/libsodium/crypto_sign/ed25519/ref10/open.c
        src/libsodium/crypto_sign/ed25519/ref10/sc_muladd.c
        src/libsodium/crypto_sign/ed25519/ref10/sc_reduce.c
        src/libsodium/crypto_sign/ed25519/ref10/sign.c
        src/libsodium/crypto_sign/edwards25519sha512batch/sign_edwards25519sha512batch_api.c
        src/libsodium/crypto_sign/edwards25519sha512batch/ref/fe25519_edwards25519sha512batch.c
        src/libsodium/crypto_sign/edwards25519sha512batch/ref/ge25519_edwards25519sha512batch.c
        src/libsodium/crypto_sign/edwards25519sha512batch/ref/sc25519_edwards25519sha512batch.c
        src/libsodium/crypto_sign/edwards25519sha512batch/ref/sign_edwards25519sha512batch.c
        src/libsodium/crypto_stream/crypto_stream.c
        src/libsodium/crypto_stream/aes128ctr/portable/afternm_aes128ctr.c
        src/libsodium/crypto_stream/aes128ctr/stream_aes128ctr_api.c
        src/libsodium/crypto_stream/aes128ctr/portable/beforenm_aes128ctr.c
        src/libsodium/crypto_stream/aes128ctr/portable/common_aes128ctr.c
        src/libsodium/crypto_stream/aes128ctr/portable/consts_aes128ctr.c
        src/libsodium/crypto_stream/aes128ctr/portable/int128_aes128ctr.c
        src/libsodium/crypto_stream/aes128ctr/portable/stream_aes128ctr.c
        src/libsodium/crypto_stream/aes128ctr/portable/xor_afternm_aes128ctr.c
        src/libsodium/crypto_stream/salsa20/stream_salsa20_api.c
        src/libsodium/crypto_stream/salsa2012/stream_salsa2012_api.c
        src/libsodium/crypto_stream/salsa2012/ref/stream_salsa2012.c
        src/libsodium/crypto_stream/salsa2012/ref/xor_salsa2012.c
        src/libsodium/crypto_stream/salsa208/stream_salsa208_api.c
        src/libsodium/crypto_stream/salsa208/ref/stream_salsa208.c
        src/libsodium/crypto_stream/salsa208/ref/xor_salsa208.c
        src/libsodium/crypto_stream/xsalsa20/stream_xsalsa20_api.c
        src/libsodium/crypto_stream/xsalsa20/ref/stream_xsalsa20.c
        src/libsodium/crypto_stream/xsalsa20/ref/xor_xsalsa20.c
        src/libsodium/crypto_verify/16/verify_16_api.c
        src/libsodium/crypto_verify/16/ref/verify_16.c
        src/libsodium/crypto_verify/32/verify_32_api.c
        src/libsodium/crypto_verify/32/ref/verify_32.c
        src/libsodium/crypto_verify/64/verify_64_api.c
        src/libsodium/crypto_verify/64/ref/verify_64.c
        src/libsodium/randombytes/randombytes.c
        src/libsodium/randombytes/salsa20/randombytes_salsa20_random.c
        src/libsodium/randombytes/sysrandom/randombytes_sysrandom.c
        src/libsodium/sodium/core.c
        src/libsodium/sodium/runtime.c
        src/libsodium/sodium/utils.c
        src/libsodium/sodium/version.c
        )

add_library(sodium STATIC ${ADD_SOURCE_SODIUM})

if("${CMAKE_CXX_COMPILER_ID}" MATCHES "MSVC")
target_compile_options(sodium PUBLIC "/analyze-")
endif()

if(${CMAKE_HOST_WIN32})
    target_link_libraries(sodium kernel32)
endif()