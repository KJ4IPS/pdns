**Note**: Beyond PowerDNS 2.9.20, the Authoritative Server and Recursor are released separately.

<!--
# PowerDNS Authoritative Server 4.1.0
Unreleased

Note: this released includes a change in the BIND zonefile parser which
affects TTLs for records that did not have an explicit TTL.  With this
change, we are compliant with RFC2308, but your existing zone files may now
be interpreted differently.

Specifically, where we previously used the SOA minimum field for the default
TTL if none was set explictly, or no $TTL was set, we now use the TTL from
the previous line.

- [#5094](https://github.com/PowerDNS/pdns/pull/5094): make our zone parser adhere to RFC2308 wrt implicit TTLs and add test
-->

# PowerDNS Recursor 4.0.5
Released 13th of June 2017

This release adds ed25519 (algorithm 15) support for DNSSEC and adds the 2017 DNSSEC root key. If you do DNSSEC validation, this upgrade is **mandatory** to continue validating after October 2017.

## Bug fixes

- [commit af76224](https://github.com/PowerDNS/pdns/commit/af76224): Correctly lowercase the TSIG algorithm name in hash computation, fixes [#4942](https://github.com/PowerDNS/pdns/issues/4942)
- [commit 86c4ed0](https://github.com/PowerDNS/pdns/commit/86c4ed0): Clear the RPZ NS IP table when clearing the policy, this prevents false positives
- [commit 5e660e9](https://github.com/PowerDNS/pdns/commit/5e660e9): Fix cache-only queries against a forward-zone, fixes [#5211](https://github.com/PowerDNS/pdns/issues/5211)
- [commit 2875033](https://github.com/PowerDNS/pdns/commit/2875033): Only delegate if NSes are below apex in auth-zones, fixes [#4771](https://github.com/PowerDNS/pdns/issues/4771)
- [commit e7c183d](https://github.com/PowerDNS/pdns/commit/e7c183d): Remove hardcoding of port 53 for TCP/IP forwarded zones in recursor, fixes [#4799](https://github.com/PowerDNS/pdns/issues/4799)
- [commit 5bec36e](https://github.com/PowerDNS/pdns/commit/5bec36e): Make sure `labelsToAdd` is not empty in `getZoneCuts()`
- [commit 0f59e05](https://github.com/PowerDNS/pdns/commit/0f59e05): Wait until after daemonizing to start the outgoing protobuf thread, prevents hangs when the protobuf server is not available
- [commit 233e144](https://github.com/PowerDNS/pdns/commit/233e144): Ensure (re)priming the root never fails
- [commit 3642cb3](https://github.com/PowerDNS/pdns/commit/3642cb3): Don't age the root, fixes a regression from 3.x
- [commit 83f9226](https://github.com/PowerDNS/pdns/commit/83f9226): Fix exception when sending a protobuf message for an empty question
- [commit ffdd813](https://github.com/PowerDNS/pdns/commit/ffdd813): LuaWrapper: Allow embedded NULs in strings received from Lua
- [commit c5ffd90](https://github.com/PowerDNS/pdns/commit/c5ffd90): Fix coredumps on illumos/SmartOS, fixes [#4579](https://github.com/PowerDNS/pdns/issues/4579) (Roman Dayneko)
- [commit 651c0e9](https://github.com/PowerDNS/pdns/commit/651c0e9): StateHolder: Allocate (and copy if needed) before taking the lock
- [commit 547d68f](https://github.com/PowerDNS/pdns/commit/547d68f): SuffixMatchNode: Fix insertion issue for an existing node
- [commit 3ada4e2](https://github.com/PowerDNS/pdns/commit/3ada4e2): Fix negative port detection for IPv6 addresses on 32-bit systems

## Additions and Enhancements

- [commit 7705e1c](https://github.com/PowerDNS/pdns/commit/7705e1c): Add support for RPZ wildcarded target names. Fixes [#5237](https://github.com/PowerDNS/pdns/issues/5237)
- [#5165](https://github.com/PowerDNS/pdns/pull/5165): Speed up RPZ zone loading and add a `zoneSizeHint` parameter to `rpzFile` and `rpzMaster` for faster reloads
- [#4794](https://github.com/PowerDNS/pdns/issues/4794): Make the RPZ summary consistent (Fixes [#4342](https://github.com/PowerDNS/pdns/issues/4342)) and log additions/removals at debug level, not info
- [commit 1909556](https://github.com/PowerDNS/pdns/commit/1909556): Add the 2017 root key
- [commit abfe671](https://github.com/PowerDNS/pdns/commit/abfe671) and [commit 7abbb2c](https://github.com/PowerDNS/pdns/commit/7abbb2c): Update Ed25519 [algorithm number and mnemonic](http://www.iana.org/assignments/dns-sec-alg-numbers/dns-sec-alg-numbers.xhtml) and hook up to the Recursor (Kees Monshouwer)
- [#5355](https://github.com/PowerDNS/pdns/pull/5355): Add `use-incoming-edns-subnet` option to process and pass along ECS and fix some ECS bugs in the process
- [commit dff1a11](https://github.com/PowerDNS/pdns/commit/dff1a11): Refuse to start with chroot set in a systemd env (Fixes [#4848](https://github.com/PowerDNS/pdns/issues/4848))
- [commit 5a38a56](https://github.com/PowerDNS/pdns/commit/5a38a56): Handle exceptions raised by `closesocket()` to prevent process termination
- [#4619](https://github.com/PowerDNS/pdns/issues/4619): Document missing `top-pub-queries` and `top-pub-servfail-queries` commands for `rec_control` (phonedph1)
- [commit 502a850](https://github.com/PowerDNS/pdns/commit/502a850): IPv6 address for g.root-servers.net added (Kevin Otte)
- [commit 7a2a645](https://github.com/PowerDNS/pdns/commit/7a2a645): Log outgoing queries / incoming responses via protobuf

# PowerDNS Authoritative Server 4.0.4
Release Candidate 1 released June 13th 2017

## Bug fixes
- [#5346](https://github.com/PowerDNS/pdns/pull/5346): configure.ac: corrects syntax error in test statement on existance of libcrypto_ecdsa (shinsterneck)
- [#5341](https://github.com/PowerDNS/pdns/pull/5341): Fix typo in ldapbackend.cc from issue #5091 (shantikulkarni)
- [#5289](https://github.com/PowerDNS/pdns/pull/5289): NSEC sorting (Kees Monshouwer)
- [#4824](https://github.com/PowerDNS/pdns/pull/4824): Check in the detected OpenSSL/libcrypto for ECDSA
- [#4781](https://github.com/PowerDNS/pdns/pull/4781): API: correctly take TTL from first record even if we are at the last comment (zeha)
- [#4901](https://github.com/PowerDNS/pdns/pull/4901): Fix AtomicCounter unit tests on 32-bit
- [#4911](https://github.com/PowerDNS/pdns/pull/4911): Fix negative port detection for IPv6 addresses on 32-bit
- [#4508](https://github.com/PowerDNS/pdns/pull/4508): Remove support for 'right' timezones, as this code turned out to be broken
- [#4961](https://github.com/PowerDNS/pdns/pull/4961): Lowercase the TSIG algorithm name in hash computation
- [#5048](https://github.com/PowerDNS/pdns/pull/5048): Handle exceptions raised by `closesocket()`
- [#5378](https://github.com/PowerDNS/pdns/pull/5378): Make sure NSEC ordernames are always lower case
- [#5297](https://github.com/PowerDNS/pdns/pull/5297):  Don't leak on signing errors during outgoing AXFR; signpipe stumbles over interrupted rrsets; fix memory leak in gmysql backend

## Improvements
- [#5325](https://github.com/PowerDNS/pdns/pull/5325): YaHTTP: Sync with upstream changes
- [#5298](https://github.com/PowerDNS/pdns/pull/5298): Notify dnsupdate backport (Kees Monshouwer)
- [#5317](https://github.com/PowerDNS/pdns/pull/5317): add option to set a global lua-axfr-script value (Kees Monshouwer)
- [#5130](https://github.com/PowerDNS/pdns/pull/5130): dnsreplay: Add `--source-ip` and `--source-port` options
- [#5085](https://github.com/PowerDNS/pdns/pull/5085): calidns: Use the correct socket family (IPv4 / IPv6)
- [#5170](https://github.com/PowerDNS/pdns/pull/5170): Backport: Add an option to allow AXFR of zones with a different (higher/lower) serial #5169  (Kees Monshouwer)
- [#5071](https://github.com/PowerDNS/pdns/pull/5071): backport #5051: fix godbc query logging (cherry-pick of d2bc6b2)
- [#4622](https://github.com/PowerDNS/pdns/pull/4622): API dot-inconsistencies
- [#4762](https://github.com/PowerDNS/pdns/pull/4762): SuffixMatchNode: Fix insertion issue for an existing node
- [#5016](https://github.com/PowerDNS/pdns/pull/5016): backport #4838: Check if we can link against libatomic if needed
- [#4861](https://github.com/PowerDNS/pdns/pull/4861): 	Do not resolve the NS-records for NOTIFY targets if the "only-notify" whitelist is empty, as a target will never match an empty whitelist.
- [#5378](https://github.com/PowerDNS/pdns/pull/5378): Improve the axfr dnssec freshness check; Ignore NSEC3PARAM metadata in an unsigned zone
- [#5297](https://github.com/PowerDNS/pdns/pull/5297): Create additional `reuseport` sockets before dropping privileges; remove transaction in pgpsql backend

# PowerDNS Authoritative Server 4.0.3
Released January 17th 2017

This release fixes an issue when using multiple backends, where one of the backends is the BIND backend.
This regression was introduced in 4.0.2.

## Bug fix

- [#4905](https://github.com/PowerDNS/pdns/pull/4905): Revert "auth: In `Bind2Backend::lookup()`, use the `zoneId` when we have it"

# PowerDNS Recursor 4.0.4
Released January 13th 2017

The 4.0.4 version of the PowerDNS Recursor fixes PowerDNS Security Advisories [2016-02](security/powerdns-advisory-2016-02.md) and [2016-04](security/powerdns-advisory-2016-04.md).

## Bug fixes

- [commit 658d9e4](https://github.com/PowerDNS/pdns/commit/658d9e4): Check TSIG signature on IXFR (Security Advisory [2016-04](security/powerdns-advisory-2016-04.md))
- [commit 91acd82](https://github.com/PowerDNS/pdns/commit/91acd82): Don't parse spurious RRs in queries when we don't need them (Security Advisory [2016-02](security/powerdns-advisory-2016-02.md))
- [commit 400e28d](https://github.com/PowerDNS/pdns/commit/400e28d): Fix incorrect length check in `DNSName` when extracting qtype or qclass
- [commit 2168188](https://github.com/PowerDNS/pdns/commit/2168188): rec: Wait until after daemonizing to start the RPZ and protobuf threads
- [commit 3beb3b2](https://github.com/PowerDNS/pdns/commit/3beb3b2): On (re-)priming, fetch the root NS records
- [commit cfeb109](https://github.com/PowerDNS/pdns/commit/cfeb109): rec: Fix src/dest inversion in the protobuf message for TCP queries
- [commit 46a6666](https://github.com/PowerDNS/pdns/commit/46a6666): NSEC3 optout and Bogus insecure forward fixes
- [commit bb437d4](https://github.com/PowerDNS/pdns/commit/bb437d4): On RPZ customPolicy, follow the resulting CNAME
- [commit 6b5a8f3](https://github.com/PowerDNS/pdns/commit/6b5a8f3): DNSSEC: don't go bogus on zero configured DSs
- [commit 1fa6e1b](https://github.com/PowerDNS/pdns/commit/1fa6e1b): Don't crash on an empty query ring
- [commit bfb7e5d](https://github.com/PowerDNS/pdns/commit/bfb7e5d): Set the result to NoError before calling `preresolve`

## Additions and Enhancements

- [commit 7c3398a](https://github.com/PowerDNS/pdns/commit/7c3398a): Add `max-recursion-depth` to limit the number of internal recursion
- [commit 3d59c6f](https://github.com/PowerDNS/pdns/commit/3d59c6f): Fix building with ECDSA support disabled in libcrypto
- [commit 0170a3b](https://github.com/PowerDNS/pdns/commit/0170a3b): Add requestorId and some comments to the protobuf definition file
- [commit d8cd67b](https://github.com/PowerDNS/pdns/commit/d8cd67b): Make the negcache forwarded zones aware
- [commit 46ccbd6](https://github.com/PowerDNS/pdns/commit/46ccbd6): Cache records for zones that were delegated to from a forwarded zone
- [commit 5aa64e6](https://github.com/PowerDNS/pdns/commit/5aa64e6), [commit 5f4242e](https://github.com/PowerDNS/pdns/commit/5f4242e) and [commit 0f707cd](https://github.com/PowerDNS/pdns/commit/0f707cd): DNSSEC: Implement keysearch based on zone-cuts
- [commit ddf6fa5](https://github.com/PowerDNS/pdns/commit/ddf6fa5): rec: Add support for boost::context >= 1.61
- [commit bb6bd6e](https://github.com/PowerDNS/pdns/commit/bb6bd6e): Add `getRecursorThreadId()` to Lua, identifying the current thread
- [commit d8baf17](https://github.com/PowerDNS/pdns/commit/d8baf17): Handle CNAMEs at the apex of secure zones to other secure zones

# PowerDNS Authoritative Server 4.0.2
Released January 13th 2017

This release fixes PowerDNS Security Advisories [2016-02](security/powerdns-advisory-2016-02.md), [2016-03](security/powerdns-advisory-2016-03.md), [2016-04](security/powerdns-advisory-2016-04.md) and [2016-05](security/powerdns-advisory-2016-05.md) and includes a fix for a memory leak in the Postgresql backend.

## Bug fixes

- [commit f61af48](https://github.com/PowerDNS/pdns/commit/f61af48): Don't parse spurious RRs in queries when we don't need them (Security Advisory [2016-02](security/powerdns-advisory-2016-02.md))
- [commit 592006d](https://github.com/PowerDNS/pdns/commit/592006d): Don't exit if the webserver can't accept a connection (Security Advisory [2016-03](security/powerdns-advisory-2016-03.md))
- [commit e85acc6](https://github.com/PowerDNS/pdns/commit/e85acc6): Check TSIG signature on IXFR (Security Advisory [2016-04](security/powerdns-advisory-2016-04.md))
- [commit 3b1e4a2](https://github.com/PowerDNS/pdns/commit/3b1e4a2): Correctly check unknown record content size (Security Advisory [2016-05](security/powerdns-advisory-2016-05.md))
- [commit 9ecbf02](https://github.com/PowerDNS/pdns/commit/9ecbf02): ODBC backend: actually prepare statements
- [commit a4d607b](https://github.com/PowerDNS/pdns/commit/a4d607b): Fix incorrect length check in `DNSName` when extracting qtype or qclass
- [commit c816fe3](https://github.com/PowerDNS/pdns/commit/c816fe3): Fix a possible memory leak in the webserver
- [#4287](https://github.com/PowerDNS/pdns/pull/4287): Better handling of invalid serial
- [#4306](https://github.com/PowerDNS/pdns/pull/4306): Limit size of mysql cell to 128 kilobytes
- [#4314](https://github.com/PowerDNS/pdns/pull/4314): Overload fix: make overload-queue-length work as intended again, add test for it.
- [#4317](https://github.com/PowerDNS/pdns/pull/4317): Improve root-zone performance
- [#4319](https://github.com/PowerDNS/pdns/pull/4319): pipe: SERVFAIL when needed
- [#4360](https://github.com/PowerDNS/pdns/pull/4360): Make sure mariadb (mysql on centos/rhel) is started before pdns (42wim)
- [#4387](https://github.com/PowerDNS/pdns/pull/4387): ComboAddress: don't allow invalid ports
- [#4459](https://github.com/PowerDNS/pdns/pull/4459): Plug memory leak in postgresql backend (Christian Hofstaedtler)
- [#4544](https://github.com/PowerDNS/pdns/pull/4544): Fix a stack-based off-by-one write in the HTTP remote backend
- [#4755](https://github.com/PowerDNS/pdns/pull/4755): calidns: Don't crash if we don't have enough 'unknown' queries remaining

## Additions and Enhancements

- [commit 1238e06](https://github.com/PowerDNS/pdns/commit/1238e06): disable negative getSOA caching if the negcache_ttl is 0 (Kees Monshouwer)
- [commit 3a0bded](https://github.com/PowerDNS/pdns/commit/3a0bded), [commit 8c879d4](https://github.com/PowerDNS/pdns/commit/8c879d4), [commit 8c03126](https://github.com/PowerDNS/pdns/commit/8c03126), [commit 5656e12](https://github.com/PowerDNS/pdns/commit/5656e12) and [commit c1d283d](https://github.com/PowerDNS/pdns/commit/c1d283d): Improve PacketCache cleaning (Kees Monshouwer)
- [#4261](https://github.com/PowerDNS/pdns/pull/4261): Strip trailing dot in PTR content (Kees Monshouwer)
- [#4269](https://github.com/PowerDNS/pdns/pull/4269): contrib: simple bash completion for pdnsutil (j0ju)
- [#4272](https://github.com/PowerDNS/pdns/pull/4272): Bind backend: update status message on reload, keep the existing zone on failure
- [#4274](https://github.com/PowerDNS/pdns/pull/4274): report DHCID type (Kees Monshouwer)
- [#4310](https://github.com/PowerDNS/pdns/pull/4310): Fix build with LibreSSL, for which OPENSSL_VERSION_NUMBER is irrelevant
- [#4323](https://github.com/PowerDNS/pdns/pull/4323): Speedup DNSName creation
- [#4335](https://github.com/PowerDNS/pdns/pull/4335): fix TSIG for single thread distributor (Kees Monshouwer)
- [#4346](https://github.com/PowerDNS/pdns/pull/4346): change default for any-to-tcp to yes (Kees Monshouwer)
- [#4356](https://github.com/PowerDNS/pdns/pull/4356): Don't look up the packet cache for TSIG-enabled queries
- [#4403](https://github.com/PowerDNS/pdns/pull/4403): (auth) Fix build with OpenSSL 1.1.0 final (Christian Hofstaedtler)
- [#4442](https://github.com/PowerDNS/pdns/pull/4442): geoipbackend: Fix minor naming issue (Aki Tuomi)
- [#4454](https://github.com/PowerDNS/pdns/pull/4454): pdnsutil: create-slave-zone accept multiple masters (Hannu Ylitalo)
- [#4541](https://github.com/PowerDNS/pdns/pull/4541): Backport of #4542: API: search should not return ENTs (Christian Hofstaedtler)
- [#4754](https://github.com/PowerDNS/pdns/pull/4754): In `Bind2Backend::lookup()`, use the `zoneId` when we have it

# PowerDNS Recursor 4.0.3
Released September 6th 2016

The 4.0.3 version of the PowerDNS Recursor features many improvements to the Policy Engine (RPZ) and the Lua bindings to it. We would like to thank Wim ([42wim](https://github.com/42wim)) for testing and reporting on the RPZ module.

## Bug fixes

- [#4350](https://github.com/PowerDNS/pdns/pull/4350): Call `gettag()` for TCP queries
- [#4376](https://github.com/PowerDNS/pdns/pull/4376): Fix the use of an uninitialized filtering policy
- [#4381](https://github.com/PowerDNS/pdns/pull/4381): Parse query-local-address before lua-config-file
- [#4383](https://github.com/PowerDNS/pdns/pull/4383): Fix accessing an empty policyCustom, policyName from Lua
- [#4387](https://github.com/PowerDNS/pdns/pull/4387): ComboAddress: don't allow invalid ports
- [#4388](https://github.com/PowerDNS/pdns/pull/4388): Fix RPZ default policy not being applied over IXFR
- [#4391](https://github.com/PowerDNS/pdns/pull/4391): DNSSEC: Actually follow RFC 7646 §2.1
- [#4396](https://github.com/PowerDNS/pdns/pull/4396): Add boost context ldflags so freebsd builds can find the libs
- [#4402](https://github.com/PowerDNS/pdns/pull/4402): Ignore NS records in a RPZ zone received over IXFR
- [#4403](https://github.com/PowerDNS/pdns/pull/4403): Fix build with OpenSSL 1.1.0 final
- [#4404](https://github.com/PowerDNS/pdns/pull/4404): Don't validate when a Lua hook took the query
- [#4425](https://github.com/PowerDNS/pdns/pull/4425): Fix a protobuf regression (requestor/responder mix-up)

## Additions and Enhancements

- [#4394](https://github.com/PowerDNS/pdns/pull/4394): Support Boost 1.61+ fcontext
- [#4402](https://github.com/PowerDNS/pdns/pull/4402): Add Lua binding for DNSRecord::d_place

# PowerDNS Recursor 4.0.2
Released August 26th 2016

This release fixes a regression in 4.x where CNAME records for DNSSEC signed domains were not sorted before the final answers, leading to some clients (notably some versions of Chrome) not being able to extract the required answer from the packet. This happened exclusively for DNSSEC signed domains, but the problem happens even for clients not requesting DNSSEC validation.

Further fixes and changes can be found below:

## Bug fixes

 - [#4264](https://github.com/PowerDNS/pdns/pull/4264): Set `dq.rcode` before calling postresolve
 - [#4294](https://github.com/PowerDNS/pdns/pull/4294): Honor PIE flags.
 - [#4310](https://github.com/PowerDNS/pdns/pull/4310): Fix build with LibreSSL, for which OPENSSL_VERSION_NUMBER is irrelevant
 - [#4340](https://github.com/PowerDNS/pdns/pull/4340): Don't shuffle CNAME records.
 - [#4354](https://github.com/PowerDNS/pdns/pull/4354): Fix delegation-only

## Additions and enhancements

 - [#4288](https://github.com/PowerDNS/pdns/pull/4288): Respect the timeout when connecting to a protobuf server
 - [#4300](https://github.com/PowerDNS/pdns/pull/4300): allow newDN to take a DNSName in; document missing methods
 - [#4301](https://github.com/PowerDNS/pdns/pull/4301): expose SMN toString to lua
 - [#4318](https://github.com/PowerDNS/pdns/pull/4318): Anonymize the protobuf ECS value as well
 - [#4324](https://github.com/PowerDNS/pdns/pull/4324): Allow Lua access to the result of the Policy Engine decision, skip RPZ, finish RPZ implementation
 - [#4349](https://github.com/PowerDNS/pdns/pull/4349): Remove unused `DNSPacket::d_qlen`
 - [#4351](https://github.com/PowerDNS/pdns/pull/4351): RPZ: Use query-local-address(6) by default
 - [#4357](https://github.com/PowerDNS/pdns/pull/4357): Move the root DNSSEC data to a header file

# PowerDNS Recursor 4.0.1
Released July 29th 2016

This release has several improvements with regards to DNSSEC validation and it improves interoperability with DNSSEC clients that expect an AD-bit on validated data when they query with only the DO-bit set.

## Bug fixes

 - [#4119](https://github.com/PowerDNS/pdns/pull/4119) Improve DNSSEC record skipping for non dnssec queries (Kees Monshouwer)
 - [#4162](https://github.com/PowerDNS/pdns/pull/4162) Don't validate zones from the local auth store, go one level down while validating when there is a CNAME
 - [#4187](https://github.com/PowerDNS/pdns/pull/4187):
   * Don't go bogus on islands of security
   * Check all possible chains for Insecures
   * Don't go Bogus on a CNAME at the apex
 - [#4215](https://github.com/PowerDNS/pdns/pull/4215) RPZ: default policy should also override local data RRs
 - [#4243](https://github.com/PowerDNS/pdns/pull/4243) Fix a crash when the next name in a chained query is empty and `rec_control current-queries` is invoked

## Improvements

 - [#4056](https://github.com/PowerDNS/pdns/pull/4056) OpenSSL 1.1.0 support (Christian Hofstaedtler)
 - [#4133](https://github.com/PowerDNS/pdns/pull/4133) Add limits to the size of received {A,I}XFR (CVE-2016-6172)
 - [#4140](https://github.com/PowerDNS/pdns/pull/4140) Fix warnings with gcc on musl-libc (James Taylor)
 - [#4160](https://github.com/PowerDNS/pdns/pull/4160) Also validate on +DO
 - [#4164](https://github.com/PowerDNS/pdns/pull/4164) Fail to start when the lua-dns-script does not exist
 - [#4168](https://github.com/PowerDNS/pdns/pull/4168) Add more Netmask methods for Lua (Aki Tuomi)
 - [#4210](https://github.com/PowerDNS/pdns/pull/4210) Validate DNSSEC for security polling
 - [#4217](https://github.com/PowerDNS/pdns/pull/4217) Turn on root-nx-trust by default and log-common-errors=off
 - [#4207](https://github.com/PowerDNS/pdns/pull/4207) Allow for multiple trust anchors per zone
 - [#4242](https://github.com/PowerDNS/pdns/pull/4242) Fix compilation warning when building without Protobuf

# PowerDNS Authoritative Server 4.0.1
Released July 29th 2016

This release fixes two small issues and adds a setting to limit AXFR and IXFR sizes, in response to [CVE-2016-6172](http://www.openwall.com/lists/oss-security/2016/07/06/4).

## Bug fixes

 - [#4126](https://github.com/PowerDNS/pdns/pull/4126) Wait for the connection to the carbon server to be established
 - [#4206](https://github.com/PowerDNS/pdns/pull/4206) Don't try to deallocate empty PG statements
 - [#4245](https://github.com/PowerDNS/pdns/pull/4245) Send the correct response when queried for an NSEC directly (Kees Monshouwer)
 - [#4252](https://github.com/PowerDNS/pdns/pull/4252) Don't include bind files if length <= 2 or > sizeof(filename)
 - [#4255](https://github.com/PowerDNS/pdns/pull/4255) Catch runtime_error when parsing a broken MNAME

## Improvements

 - [#4044](https://github.com/PowerDNS/pdns/pull/4044) Make DNSPacket return a ComboAddress for local and remote (Aki Tuomi)
 - [#4056](https://github.com/PowerDNS/pdns/pull/4056) OpenSSL 1.1.0 support (Christian Hofstaedtler)
 - [#4169](https://github.com/PowerDNS/pdns/pull/4169) Fix typos in a logmessage and exception (Christian Hofstaedtler)
 - [#4183](https://github.com/PowerDNS/pdns/pull/4183) pdnsutil: Remove checking of ctime and always diff the changes (Hannu Ylitalo)
 - [#4192](https://github.com/PowerDNS/pdns/pull/4192) dnsreplay: Only add Client Subnet stamp when asked
 - [#4250](https://github.com/PowerDNS/pdns/pull/4250) Use toLogString() for ringAccount (Kees Monshouwer)

## Additions

 - [#4133](https://github.com/PowerDNS/pdns/pull/4133) Add limits to the size of received {A,I}XFR (CVE-2016-6172)
 - [#4142](https://github.com/PowerDNS/pdns/pull/4142) Add used filedescriptor statistic (Kees Monshouwer)

# PowerDNS Recursor 4.0.0
Released July 11th 2016

PowerDNS Recursor 4.0.0 is part of [the great 4.x "Spring Cleaning"](http://blog.powerdns.com/2015/11/28/powerdns-spring-cleaning/) of PowerDNS which lasted through the end of 2015.

As part of the general cleanup, we did the following:

- Moved to C++ 2011, a cleaner more powerful version of C++ that has allowed us to [improve the quality of implementation](http://bert-hubert.blogspot.nl/2015/01/on-c2011-quality-of-implementation.html) in many places.
- Implemented dedicated infrastructure for dealing with DNS names that is fully "DNS Native" and needs less escaping and unescaping
- Switched to binary storage of DNS records in all places
- Moved ACLs to a dedicated Netmask Tree
- Implemented a version of [RCU](https://en.wikipedia.org/wiki/Read-copy-update) for configuration changes
- Instrumented our use of the memory allocator, reduced number of malloc calls substantially.
- The Lua hook infrastructure was redone using LuaWrapper; old scripts will no longer work, but new scripts are easier to write under the new interface.

In addition to this cleanup, which has many internal benefits and solves longstanding issues with escaped domain names, 4.0.0 brings the following major new features:

- RPZ aka Response Policy Zone support
- IXFR slaving in the PowerDNS Recursor for RPZ
- DNSSEC processing in Recursor (Authoritative has had this for years)
- DNSSEC validation (without NSEC(3) proof validation)
- EDNS Client Subnet support in PowerDNS Recursor (Authoritative has had this for years)
- Lua asynchronous queries for per-IP/per-domain status
- Caches that can now be wiped per whole zone instead of per name
- Statistics on authoritative server response times (split for IPv4 and IPv6)
- APIs are no longer marked as 'experimental' and had one final URL change
- New metric: tcp-answer-bytes to measure DNS TCP/IP bandwidth, and many other new metrics

Please be aware that beyond the items listed here, there have been heaps of tiny changes. As always, please carefully test a new release before deploying it.

This release features the following fixes compared to rc1:

 - [#3989](https://github.com/PowerDNS/pdns/pull/3989) Fix usage of std::distance() in DNSName::isPartOf() (signed/unsigned comparisons)
 - [#4017](https://github.com/PowerDNS/pdns/pull/4017) Fix building without Lua. Add `isTcp` to `dq`.
 - [#4023](https://github.com/PowerDNS/pdns/pull/4023) Actually log on dnssec=log-fail
 - [#4028](https://github.com/PowerDNS/pdns/pull/4028) DNSSEC fixes (NSEC casing, send DO-bit over TCP, DNSSEC trace additions)
 - [#4052](https://github.com/PowerDNS/pdns/pull/4052) Don't fail configure on missing fcontext.hpp
 - [#4096](https://github.com/PowerDNS/pdns/pull/4096) Don't call `commit()` if we skipped all the records

It has the following improvements:

 - [#3400](https://github.com/PowerDNS/pdns/pull/3400) Enable building on OpenIndiana
 - [#4016](https://github.com/PowerDNS/pdns/pull/4016) Log protobuf messages for cache hits. Add policy tags in gettag()
 - [#4040](https://github.com/PowerDNS/pdns/pull/4040) Allow DNSSEC validation when chrooted
 - [#4094](https://github.com/PowerDNS/pdns/pull/4094) Sort included html files for improved reproducibility (Christian Hofstaedtler)

And these additions:

 - [#3981](https://github.com/PowerDNS/pdns/pull/3981) Import JavaScript sources for libs shipped with Recursor (Christian Hofstaedtler)
 - [#4012](https://github.com/PowerDNS/pdns/pull/4012) add tags support to ProtobufLogger.py
 - [#4032](https://github.com/PowerDNS/pdns/pull/4032) Set the existing policy tags in `dq` for `{pre,post}resolve`
 - [#4077](https://github.com/PowerDNS/pdns/pull/4077) Add DNSSEC validation statistics
 - [#4090](https://github.com/PowerDNS/pdns/pull/4090) Allow reloading the lua-config-file at runtime
 - [#4097](https://github.com/PowerDNS/pdns/pull/4097) Allow logging DNSSEC bogus in any mode
 - [#4125](https://github.com/PowerDNS/pdns/pull/4125) Add protobuf fields for the query's time in the response

## PowerDNS Recursor 4.0.0-rc1
Released June 9th 2016

This first (and hopefully last) Release Candidate contains the finishing touches
to the experimental DNSSEC support by adding (Negative) Trust Anchor support and
fixing a possible issue with DNSSEC and forwarded domains:

- [#3910](https://github.com/PowerDNS/pdns/pull/3910) Add (Negative) Trust Anchor management
- [#3926](https://github.com/PowerDNS/pdns/pull/3926) Set +CD on forwarded recursive queries

Other changes:

- [#3941](https://github.com/PowerDNS/pdns/pull/3941) Ensure delegations from local auth zones are followed
- [#3924](https://github.com/PowerDNS/pdns/pull/3924) Add a virtual hosting unit-file
- [#3929](https://github.com/PowerDNS/pdns/pull/3929) Set the FDs in the unit file to a sane value

Bug fixes:

- [#3961](https://github.com/PowerDNS/pdns/pull/3961) Fix building on EL6 i386
- [#3957](https://github.com/PowerDNS/pdns/pull/3957) Add error reporting when parsing forward-zones(-recurse) (Aki Tuomi)

## PowerDNS Recursor 4.0.0-beta1
Released May 27th 2016

This release fixes a bug in the DNSSEC implementation where a name would we validated as bogus when talking to non-compliant authoritative servers:

- [#3875](https://github.com/PowerDNS/pdns/pull/3875) Disable DNSSEC for domain where the auth responds with FORMERR or NOTIMP

## Improvements

- [#3866](https://github.com/PowerDNS/pdns/pull/3866) Increase max FDs in systemd unit file
- [#3905](https://github.com/PowerDNS/pdns/pull/3905) Add a dnssec=process-no-validate option and make it default

## Bug fixes

- [#3881](https://github.com/PowerDNS/pdns/pull/3881) Fix the `noEdnsOutQueries` counter
- [#3892](https://github.com/PowerDNS/pdns/pull/3892) support `clock_gettime` for platforms that require -lrt

## PowerDNS Recursor 4.0.0-alpha3
Released May 10th 2016

This release features several leaps in the correctness and stability of the DNSSEC implementation.

Notable changes are:

- [#3752](https://github.com/PowerDNS/pdns/pull/3752) Correct handling of query flags in conformance with [RFC 6840](https://tools.ietf.org/html/rfc6840)

## Bug fixes

- [#3804](https://github.com/PowerDNS/pdns/pull/3804) Fix a memory leak in DNSSEC validation
- [#3785](https://github.com/PowerDNS/pdns/pull/3785) and [#3390](https://github.com/PowerDNS/pdns/pull/3390) Correctly validate insecure delegations
- [#3606](https://github.com/PowerDNS/pdns/pull/3606) Various DNSSEC fixes, disabling DNSSEC on forward-zones
- [#3681](https://github.com/PowerDNS/pdns/pull/3681) Catch exception with a malformed DNSName in `rec_control wipe-cache`
- [#3779](https://github.com/PowerDNS/pdns/pull/3779), [#3768](https://github.com/PowerDNS/pdns/pull/3768), [#3766](https://github.com/PowerDNS/pdns/pull/3766), [#3783](https://github.com/PowerDNS/pdns/pull/3783) and [#3789](https://github.com/PowerDNS/pdns/pull/3789) DNSName and other hardening improvements

## Improvements

- [#3801](https://github.com/PowerDNS/pdns/pull/3801) Add missing Lua rcodes bindings
- [#3587](https://github.com/PowerDNS/pdns/pull/3587) Update L-Root addresses

## PowerDNS Recursor 4.0.0-alpha2
Released March 9th 2016

Note that the DNSSEC implementation has several bugs in this release, it is advised to set `dnssec=off` in your recursor.conf.

This release features many low-level performance fixes. Other notable changes since 4.0.0-alpha1 are:

- [#3259](https://github.com/PowerDNS/pdns/pull/3259), [#3280](https://github.com/PowerDNS/pdns/pull/3280) The PowerDNS Recursor now properly uses GNU autoconf and autotools for building and installing
- OpenSSL crypto primitives are now used for DNSSEC validation
- [#3313](https://github.com/PowerDNS/pdns/pull/3313) Implement the logic we need to generate EDNS MAC fields in dnsdist & read them in recursor ([blogpost](http://blog.powerdns.com/2016/01/27/per-device-dns-settings-selective-parental-control/)
- [#3350](https://github.com/PowerDNS/pdns/pull/3350) Add lowercase-outgoing feature to Recursor
- [#3410](https://github.com/PowerDNS/pdns/pull/3410) Recuweb is now built-in to the daemon
- [#3230](https://github.com/PowerDNS/pdns/pull/3230) API: drop JSONP, add web security headers (Christian Hofstaedtler)
- [#3485](https://github.com/PowerDNS/pdns/pull/3485) Allow multiple carbon-servers
- [#3427](https://github.com/PowerDNS/pdns/pull/3427), [#3479](https://github.com/PowerDNS/pdns/pull/3479), [#3472](https://github.com/PowerDNS/pdns/pull/3472) MTasker modernization (Andrew Nelless)

### Bug fixes

- [#3444](https://github.com/PowerDNS/pdns/pull/3444), [#3442](https://github.com/PowerDNS/pdns/pull/3442) RPZ IXFR fixes
- [#3448](https://github.com/PowerDNS/pdns/pull/3448) Remove edns-subnet-whitelist whitelist pointing to powerdns.com (Christian Hofstaedtler)
- [#3293](https://github.com/PowerDNS/pdns/pull/3293) make asynchronous UDP Lua queries work again in 4.x
- [#3365](https://github.com/PowerDNS/pdns/pull/3365) Apply rcode set in UDPQueryResponse callback (Jan Broers)
- [#3244](https://github.com/PowerDNS/pdns/pull/3244) Fix the forward zones in the recursor
- [#3135](https://github.com/PowerDNS/pdns/pull/3135) Use 56 bits instead of 64 in EDNS Client Subnet option (Winfried Angele)
- [#3527](https://github.com/PowerDNS/pdns/pull/3527) Make the recursor counters atomic

### Improvements

- [#3435](https://github.com/PowerDNS/pdns/pull/3435) Add `toStringNoDot` and `chopOff` functions to Lua
- [#3437](https://github.com/PowerDNS/pdns/pull/3437) Add `pdns.now` timeval struct to recursor Lua
- [#3352](https://github.com/PowerDNS/pdns/pull/3352) Cache improvements
- [#3502](https://github.com/PowerDNS/pdns/pull/3502) Make second argument to pdnslog optional (Thiago Farina)
- [#3520](https://github.com/PowerDNS/pdns/pull/3520) Reduce log level of periodic statistics to notice (Jan Broers)

## PowerDNS Recursor 4.0.0-alpha1
Released December 24th 2015

# PowerDNS Authoritative Server 4.0.0
Released July 11th 2016

PowerDNS Authoritative Server 4.0.0 is part of [the great 4.x "Spring Cleaning"](http://blog.powerdns.com/2015/11/28/powerdns-spring-cleaning/)
of PowerDNS which lasted through the end of 2015.

As part of the general cleanup and improvements, we did the following:

- Moved to C++ 2011, a cleaner more powerful version of C++ that has allowed us to [improve the quality of implementation](http://bert-hubert.blogspot.nl/2015/01/on-c2011-quality-of-implementation.html) in many places.
- Implemented dedicated infrastructure for dealing with DNS names that is fully "DNS Native" and needs less escaping and unescaping.
- All backends derived from the Generic SQL backend use [prepared statements](authoritative/backend-generic-sql.md).
- Both the server and `pdns_control` do the right thing when `chroot`'ed.

In addition to this cleanup, 4.0.0 brings the following new features:

- A revived ODBC backend ([godbc](authoritative/backend-generic-odbc.md)).
- A revived LDAP backend ([ldap](authoritative/backend-ldap.md)).
- Support for [CDS/CDNSKEY](authoritative/howtos.md#cds-cdnskey-key-rollover) and [RFC 7344](https://tools.ietf.org/html/rfc7344) key-rollovers.
- Support for the [ALIAS](authoritative/howtos.md#using-alias-records) record.
- The webserver and API are no longer marked experimental.
    - The API-path has moved to `/api/v1`
- DNSUpdate is no longer experimental.
- Default ECDSA (algorithms 13 and 14) support without external dependencies.
- Experimental support for ed25519 DNSSEC signatures (when compiled with libsodium support).
- IXFR consumption support.
- Many new `pdnsutil` commands
    - `help` command now produces the help
    - Warns if the configuration file cannot be read
    - Does not check disabled records with `check-zone` unless verbose mode is enabled
    - `create-zone` command creates a new zone
    - `add-record` command to add records
    - `delete-rrset` and `replace-rrset` commands to delete and add rrsets
    - `edit-zone` command that spawns `$EDITOR` with the zone contents in zonefile format regardless of the backend used ([blogpost](http://blog.powerdns.com/2016/02/02/powerdns-authoritative-the-new-old-way-to-manage-domains/)

The following backend have been dropped in 4.0.0:

- LMDB.
- Geo (use the improved [GeoIP](authoritative/backend-geoip.md) instead).

Important changes:

- `pdnssec` has been renamed to `pdnsutil`
- PowerDNS Authoritative Server now listens by default on all IPv6 addresses.
- The default for `pdnsutil secure-zone` has been changed from 1 2048 bit RSA KSK and 1 1024 bit RSA ZSK to a single 256 bit ECDSA (algorithm 13, ECDSAP256SHA256) key.
- Several superfluous queries have been dropped from the SQL backend, if you use a non-standard SQL schema, please review the new defaults
    - `insert-ent-query`, `insert-empty-non-terminal-query`, `insert-ent-order-query` have been replaced by one query named `insert-empty-non-terminal-order-query`
    - `insert-record-order-query` has been dropped, `insert-record-query` now sets the ordername (or NULL)
    - `insert-slave-query` has been dropped, `insert-zone-query` now sets the type of zone
- Crypto++ and mbedTLS support is dropped, these are replaced by OpenSSL
- The INCEPTION, INCEPTION-WEEK and EPOCH SOA-EDIT metadata values are marked as deprecated and will be removed in 4.1

The final release has the following bug fixes compared to rc2:

 - [#4071](https://github.com/PowerDNS/pdns/pull/4071) Abort on backend failures at startup and retry while running (Kees Monshouwer)
 - [#4099](https://github.com/PowerDNS/pdns/pull/4099) Don't leak TCP connection descriptor if `pthread_create()` failed
 - [#4137](https://github.com/PowerDNS/pdns/pull/4137) gsqlite3: Check whether foreign keys should be turned on (Aki Tuomi)

And the following improvements:

 - [#3051](https://github.com/PowerDNS/pdns/pull/3051) Better error message for unfound new slave domains
 - [#4123](https://github.com/PowerDNS/pdns/pull/4123) check-zone: warn on mismatch between algo and NSEC mode

## PowerDNS Authoritative Server 4.0.0-rc2
Released June 29th 2016

**note**: rc1 was tagged in git but never officially released.
Kees Monshouwer discovered an issue in the gmysql backend that would terminate the daemon on a connection error, this fixed in rc2.

This Release Candidate adds IXFR consumption and fixes some issues with prepared statements:

 - [#3937](https://github.com/PowerDNS/pdns/pull/3937) GSQL: use lazy prepared statements (Aki Tuomi)
 - [#3949](https://github.com/PowerDNS/pdns/pull/3949) Implement IXFR-based slaving for Authoritative, fix duplicate AXFRs
 - [#4066](https://github.com/PowerDNS/pdns/pull/4066) Don't die on a mysql timeout (Kees Monshouwer)

Other improvements:

 - [#4061](https://github.com/PowerDNS/pdns/pull/4061) Various fixes, a MySQL-query fix that improves performance and one that allows shorter best matches in getAuth()
 - [#3962](https://github.com/PowerDNS/pdns/pull/3962) Fix OpenBSD support
 - [#3972](https://github.com/PowerDNS/pdns/pull/3972) API: change PATCH/PUT on zones to return 204 No Content instead of full zone (Christian Hofstaedtler)
 - [#3917](https://github.com/PowerDNS/pdns/pull/3917) Remotebackend: Add getAllDomains call (Aki Tuomi)

Bug fixes and changes:

 - [#3998](https://github.com/PowerDNS/pdns/pull/3998) remove gsql::isOurDomain for now (Kees Monshouwer)
 - [#3989](https://github.com/PowerDNS/pdns/pull/3989) Fix usage of std::distance() in DNSName::isPartOf()
 - [#4001](https://github.com/PowerDNS/pdns/pull/4001) re enable validDNSName() check (Kees Monshouwer)
 - [#3930](https://github.com/PowerDNS/pdns/pull/3930) Have pdns_control bind-add-zone check for zonefile
 - [#3400](https://github.com/PowerDNS/pdns/pull/3400) Fix building on OpenIndiana
 - [#3961](https://github.com/PowerDNS/pdns/pull/3961) Allow building on CentOS 6 i386
 - [#3940](https://github.com/PowerDNS/pdns/pull/3940) auth: Don't build dnsbulktest and dnstcpbench if boost is too old, fixes building on CentOS 6
 - [#3931](https://github.com/PowerDNS/pdns/pull/3931) Rename `notify` to `pdns_notify` (Christian Hofstaedtler)

## PowerDNS Authoritative Server 4.0.0-beta1
Released May 27th 2016

This release features several small fixes and deprecations.

## Improvements and Additions

- [#3851](https://github.com/PowerDNS/pdns/pull/3851) Disable algorithm 13 and 14 if OpenSSL does not support ecdsa or the required curves (Kees Monshouwer)
- [#3857](https://github.com/PowerDNS/pdns/pull/3857) Add simple stubquery tool for testing the stubresolver
- [#3859](https://github.com/PowerDNS/pdns/pull/3859) build scripts: Stop patching config-dir in pdns.conf (Christian Hofstaedtler)
- [#3872](https://github.com/PowerDNS/pdns/pull/3872) Add support for multiple carbon servers
- [#3901](https://github.com/PowerDNS/pdns/pull/3901) Add support for virtual hosting with systemd

## Bug fixes

- [#3856](https://github.com/PowerDNS/pdns/pull/3856) Deal with unset name in nproxy replies

## PowerDNS Authoritative Server 4.0.0-alpha3
Released May 11th 2016

Notable changes since 4.0.0-alpha2

- [#3415](https://github.com/PowerDNS/pdns/pull/3415) pdnsutil: add clear-zone command
- [#3586](https://github.com/PowerDNS/pdns/pull/3586) Remove send-root-referral option
- [#3578](https://github.com/PowerDNS/pdns/pull/3578) Add disable-syslog option
- [#3733](https://github.com/PowerDNS/pdns/pull/3733) ALIAS improvements: DNSSEC and optional on-AXFR expansion of records
- [#3764](https://github.com/PowerDNS/pdns/pull/3764) Notify support for systemd
- [#3807](https://github.com/PowerDNS/pdns/pull/3807) Add TTL settings for DNSSECKeeper's caches

### Bug fixes

- [#3553](https://github.com/PowerDNS/pdns/pull/3553) pdnsutil: properly show key sizes for presigned zones in show-zone
- [#3507](https://github.com/PowerDNS/pdns/pull/3507) webserver: mask out the api-key setting (Christian Hofstaedtler)
- [#3580](https://github.com/PowerDNS/pdns/pull/3580) bindbackend: set domain in list() (Kees Monshouwer)
- [#3595](https://github.com/PowerDNS/pdns/pull/3595) pdnsutil: add NS record without trailing dot with create-zone
- [#3653](https://github.com/PowerDNS/pdns/pull/3653) Allow tabs as whitespace in zonefiles
- [#3666](https://github.com/PowerDNS/pdns/pull/3666) Restore recycle backend behaviour (Kees Monshouwer)
- [#3612](https://github.com/PowerDNS/pdns/pull/3612) Prevent segfault in PostgreSQL backend
- [#3779](https://github.com/PowerDNS/pdns/pull/3779), [#3768](https://github.com/PowerDNS/pdns/pull/3768), [#3766](https://github.com/PowerDNS/pdns/pull/3766), [#3783](https://github.com/PowerDNS/pdns/pull/3783) and [#3789](https://github.com/PowerDNS/pdns/pull/3789) DNSName and other hardening improvements
- [#3784](https://github.com/PowerDNS/pdns/pull/3784) fix SOA caching with multiple backends (Kees Monshouwer)
- [#3827](https://github.com/PowerDNS/pdns/pull/3827) Force NSEC3PARAM algorithm to 1, fixes validation issues when set to not 1

### Improvements

- [#3637](https://github.com/PowerDNS/pdns/pull/3637), [#3678](https://github.com/PowerDNS/pdns/pull/3678), [#3740](https://github.com/PowerDNS/pdns/pull/3740) Correct root-zone slaving and serving (Kees Monshouwer and others)
- [#3495](https://github.com/PowerDNS/pdns/pull/3495) API: Add discovery endpoint (Christian Hofstaedtler)
- [#3389](https://github.com/PowerDNS/pdns/pull/3389) pdnsutil: support chroot
- [#3596](https://github.com/PowerDNS/pdns/pull/3596) Remove botan-based ecdsa and rsa signers (Kees Monshouwer)
- [#3478](https://github.com/PowerDNS/pdns/pull/3478), [#3603](https://github.com/PowerDNS/pdns/pull/3603), [#3628](https://github.com/PowerDNS/pdns/pull/3628) Various build system improvements (Ruben Kerkhof)
- [#3621](https://github.com/PowerDNS/pdns/pull/3621) Always lowercase when inserting into the database
- [#3651](https://github.com/PowerDNS/pdns/pull/3651) Rename PUBLISH\_\* to PUBLISH-\* domainmetadata
- [#3656](https://github.com/PowerDNS/pdns/pull/3656) API: clean up cryptokeys resource (Christian Hofstaedtler)
- [#3632](https://github.com/PowerDNS/pdns/pull/3632) pdnsutil: Fix exit statuses to constants and return 0 when success (saltsa)
- [#3655](https://github.com/PowerDNS/pdns/pull/3655) API: Fix set-ptr to honor SOA-EDIT-API (Christian Hofstaedtler)
- [#3720](https://github.com/PowerDNS/pdns/pull/3720) Many fixes for dnswasher (Robert Edmonds)
- [#3707](https://github.com/PowerDNS/pdns/pull/3707), [#3788](https://github.com/PowerDNS/pdns/pull/3788) Make MySQL timeout configurable (Kees Monshouwer and Brynjar Eide)
- [#3806](https://github.com/PowerDNS/pdns/pull/3806) Move key validity check out of `fromISCMap()`, improves DNSSEC performance
- [#3820](https://github.com/PowerDNS/pdns/pull/3820) pdnsutil load-zone: ignore double SOA

## PowerDNS Authoritative Server 4.0.0-alpha2
Released February 25th 2016

Notable changes since 4.0.0-alpha1

- [#3037](https://github.com/PowerDNS/pdns/pull/3037) Remove superfluous gsql queries and stop relying on schema defaults
- [#3176](https://github.com/PowerDNS/pdns/pull/3176), [#3139](https://github.com/PowerDNS/pdns/pull/3139) OpenSSL support (Christian Hofstaedtler and Kees Monshouwer)
- [#3128](https://github.com/PowerDNS/pdns/pull/3128) ECDSA support to DNSSEC infra via OpenSSL (Kees Monshouwer)
- [#3281](https://github.com/PowerDNS/pdns/pull/3281), [#3283](https://github.com/PowerDNS/pdns/pull/3283), [#3363](https://github.com/PowerDNS/pdns/pull/3363) Remove Crypto++ and mbedTLS support
- [#3298](https://github.com/PowerDNS/pdns/pull/3298) Implement pdnsutil create-zone zone nsname, add-record, delete-rrset, replace-rrset
- [#3407](https://github.com/PowerDNS/pdns/pull/3407) API: Permit wildcard manipulation  (Aki Tuomi)
- [#3230](https://github.com/PowerDNS/pdns/pull/3230) API: drop JSONP, add web security headers (Christian Hofstaedtler)
- [#3428](https://github.com/PowerDNS/pdns/pull/3428) API: Fix zone/records design mistake (Christian Hofstaedtler)
    - **Note**: this is a breaking change from alpha1, please review the [API documentation](httpapi/api_spec.md)

### Bug fixes

- [#3124](https://github.com/PowerDNS/pdns/pull/3124) Fix several bugs with introduced with the change to a single signing key (e.g. the SEP bit is set on these single keys)
- [#3151](https://github.com/PowerDNS/pdns/pull/3151) Catch DNSName build errors in dynhandler (Christian Hofstaedtler)
- [#3264](https://github.com/PowerDNS/pdns/pull/3264) GeoIP backend: Use correct id numbers for domains (Aki Tuomi)
- [#3271](https://github.com/PowerDNS/pdns/pull/3271) ZoneParser: Throw PDNSException on too many SOA data elements
- [#3302](https://github.com/PowerDNS/pdns/pull/3302) Fix bindbackend's feedRecord to handle being slave for the root
- [#3399](https://github.com/PowerDNS/pdns/pull/3399) Report OpenSSL RSA keysize in bits (Kees Monshouwer)

### Improvements

- [#3119](https://github.com/PowerDNS/pdns/pull/3119) Show DNSSEC keys for slaved zone (Aki Tuomi)
- [#3255](https://github.com/PowerDNS/pdns/pull/3255) Don't log authentication errors before sending HTTP basic auth challenge (Jan Broer)
- [#3338](https://github.com/PowerDNS/pdns/pull/3338) Add weight feature to GeoIP backend (Aki Tuomi)
- [#3364](https://github.com/PowerDNS/pdns/pull/3364) Shrink PacketID by 10% by eliminating padding. (Andrew Nelless)
- [#3443](https://github.com/PowerDNS/pdns/pull/3443) Many speedup and correctness fixes

## PowerDNS Authoritative Server 4.0.0-alpha1
Released December 24th 2015


# PowerDNS Authoritative Server 3.4.9
Released 17th of May 2016

This is a minor bugfix and performance release. Two contributions by Kees Monshouwer make 3.4.9 fully compatible with the new single key ECDSA default that is coming in version 4.0.0.

Changes since 3.4.8:

- [commit 4627ea0](https://github.com/PowerDNS/pdns/commit/4627ea0), [commit 8350828](https://github.com/PowerDNS/pdns/commit/8350828): use OpenSSL for ECDSA signing where available (Kees Monshouwer)
- [commit 558ff84](https://github.com/PowerDNS/pdns/commit/558ff84): allow common signing key (Kees Monshouwer)
- [commit 280d665](https://github.com/PowerDNS/pdns/commit/280d665): Add a disable-syslog setting
- [commit 58d6ab6](https://github.com/PowerDNS/pdns/commit/58d6ab6): fix SOA caching with multiple backends (Kees Monshouwer)
- [commit e9e413f](https://github.com/PowerDNS/pdns/commit/e9e413f), [commit 6af4652](https://github.com/PowerDNS/pdns/commit/6af4652): whitespace-related zone parsing fixes [ticket #3568](https://github.com/PowerDNS/pdns/issues/3568)
- [commit 7473a5e](https://github.com/PowerDNS/pdns/commit/7473a5e): bindbackend: fix, set domain in list() (Kees Monshouwer)

# PowerDNS Authoritative Server 3.4.8
Released 3rd of February 2016

This is a small bugfix release. Additionally, the deb/RPM packages on downloads.powerdns.com (those with -static in the name) for 3.4.8 have been built against Botan 1.10.11 instead of Botan 1.10.3 like previous packages. Please see [the Botan Security page](http://botan.randombit.net/security.html) for more information on the fixes in Botan 1.10.11. As a PowerDNS user, these issues only affect you if you ran our -static packages *and* allowed your users to upload private keys to your configuration.

Changes since 3.4.7:

- [commit edfa60a](https://github.com/PowerDNS/pdns/commit/edfa60a): Use AC_SEARCH_LIBS (Ruben Kerkhof)
- [commit 7b7a3af](https://github.com/PowerDNS/pdns/commit/7b7a3af): Check for inet_aton in libresolv (Ruben Kerkhof)
- [commit 9322aee](https://github.com/PowerDNS/pdns/commit/9322aee): Remove hardcoded -lresolv, -lnsl and -lsocket (Ruben Kerkhof)
- [commit 23d26d8](https://github.com/PowerDNS/pdns/commit/23d26d8): pdnssec: don't check disabled records (Pieter Lexis)
- [commit ce92ff1](https://github.com/PowerDNS/pdns/commit/ce92ff1): pdnssec: check all records (including disabled ones) only in verbose mode (Kees Monshouwer)
- [commit f745312](https://github.com/PowerDNS/pdns/commit/f745312): trailing dot in DNAME content (Kees Monshouwer)
- [commit ed02761](https://github.com/PowerDNS/pdns/commit/ed02761): Fix luabackend compilation on FreeBSD i386 (RvdE)
- [commit 07ea6ac](https://github.com/PowerDNS/pdns/commit/07ea6ac): silence g++ 6.0 warnings and error (Kees Monshouwer)
- [commit c6077b1](https://github.com/PowerDNS/pdns/commit/c6077b1): add gcc 5.3 and 6.0 support to boost.m4 (Kees Monshouwer)

# PowerDNS Authoritative Server 3.4.7
Released 3rd of November 2015

This is a security release fixing [Security Advisory
2015-03](security/powerdns-advisory-2015-03.md)

Bug fixes:

- [commit b0c04ba](https://github.com/PowerDNS/pdns/commit/b0c04ba): Ignore invalid/empty TKEY and TSIG records (Christian Hofstaedtler)
- [commit 8044a5d](https://github.com/PowerDNS/pdns/commit/8044a5d): Don't reply to truncated queries (Christian Hofstaedtler)
- [commit 6a65ae9](https://github.com/PowerDNS/pdns/commit/6a65ae9): don't log out-of-zone ents during AXFR in (Kees Monshouwer)
- [commit 416d252](https://github.com/PowerDNS/pdns/commit/416d252): Prevent XSS by escaping user input. Thanks to Pierre Jaury and Damien Cauquil at Sysdream for pointing this out.
- [commit df76bda](https://github.com/PowerDNS/pdns/commit/df76bda): Handle NULL and boolean properly in gPGSql (Aki Tuomi)
- commits [b998fc0](https://github.com/PowerDNS/pdns/commit/b998fc0),
  [88516fd](https://github.com/PowerDNS/pdns/commit/88516fd),
  [ef80925](https://github.com/PowerDNS/pdns/commit/ef80925),
  [4549a72](https://github.com/PowerDNS/pdns/commit/4549a72): Improve negative caching (Kees Monshouwer)
- [commit be27a9c](https://github.com/PowerDNS/pdns/commit/be27a9c): Do not divide timeout twice (Aki Tuomi)
- commits [ca1d29c](https://github.com/PowerDNS/pdns/commit/ca1d29c),
  [df2d20a](https://github.com/PowerDNS/pdns/commit/df2d20a),
  [2358eea](https://github.com/PowerDNS/pdns/commit/2358eea): Correctly sort records with a priority.


Improvements:

- commits [791bc37](https://github.com/PowerDNS/pdns/commit/791bc37),
  [e3301ca](https://github.com/PowerDNS/pdns/commit/e3301ca),
  [9862779](https://github.com/PowerDNS/pdns/commit/9862779),
  [b59a7e3](https://github.com/PowerDNS/pdns/commit/b59a7e3),
  [4ca7a06](https://github.com/PowerDNS/pdns/commit/4ca7a06),
  [7736530](https://github.com/PowerDNS/pdns/commit/7736530),
  [69ea1a6](https://github.com/PowerDNS/pdns/commit/69ea1a6): Direct query answers and correct zone-rectification in the GeoIP backend (Aki Tuomi)
- commits [83e0e53](https://github.com/PowerDNS/pdns/commit/83e0e53),
  [0ff3037](https://github.com/PowerDNS/pdns/commit/0ff3037),
  [9910908](https://github.com/PowerDNS/pdns/commit/9910908) Use token names to identify PKCS#11 keys (Aki Tuomi)
- [commit a3801b2](https://github.com/PowerDNS/pdns/commit/a3801b2): Fix typo in an error message (Arjen Zonneveld)
- [commit d33ba8e](https://github.com/PowerDNS/pdns/commit/d33ba8e): limit NSEC3 iterations in bindbackend (Kees Monshouwer)
- [commit 0acca87](https://github.com/PowerDNS/pdns/commit/0acca87): Initialize minbody (Aki Tuomi)


New features:

- commits [4d51e96](https://github.com/PowerDNS/pdns/commit/4d51e96),
  [6873a07](https://github.com/PowerDNS/pdns/commit/6873a07),
  [b972356](https://github.com/PowerDNS/pdns/commit/b972356),
  [46294b5](https://github.com/PowerDNS/pdns/commit/46294b5),
  [6277b14](https://github.com/PowerDNS/pdns/commit/6277b14): OPENPGPKEY record-type (James Cloos and Kees Monshouwer)
- [commit ec0ded7](https://github.com/PowerDNS/pdns/commit/ec0ded7): add global soa-edit settings (Kees Monshouwer)

# PowerDNS Authoritative Server 3.4.6
Released 28th of August 2015

This is a security release fixing [Security Advisory
2015-02](security/powerdns-advisory-2015-02.md)

Bug fixes:

- commits [c849701](https://github.com/PowerDNS/pdns/commit/c849701) and
[8c91e2c](https://github.com/PowerDNS/pdns/commit/8c91e2c): Avoid
superfluous backend recycling
- commits [463fcff](https://github.com/PowerDNS/pdns/commit/463fcff),
[0fc08e8](https://github.com/PowerDNS/pdns/commit/0fc08e8),
[0fbe69c](https://github.com/PowerDNS/pdns/commit/0fbe69c),
[1a6af1c](https://github.com/PowerDNS/pdns/commit/1a6af1c) and
[07f69d3](https://github.com/PowerDNS/pdns/commit/07f69d3): Removal of
dnsdist from the authoritative server distribution (Kees Monshouwer among others).
- commits [5cfea4c](https://github.com/PowerDNS/pdns/commit/5cfea4c) and
[ef011d9](https://github.com/PowerDNS/pdns/commit/ef011d9): Add EDNS
unknown version handling and tests EDNS unknown version handling (Aki Tuomi)

Improvements:

- commits [88dd8a7](https://github.com/PowerDNS/pdns/commit/88dd8a7) and
[dc6c63d](https://github.com/PowerDNS/pdns/commit/dc6c63d): Update
YaHTTP to v0.1.7 (Aki Tuomi)
- [commit 0a344bc](https://github.com/PowerDNS/pdns/commit/0a344bc): Make
trailing/leading spaces stand out in `pdnssec check_zone`
- commits [2e982ad](https://github.com/PowerDNS/pdns/commit/2e982ad) and
[09bec1f](https://github.com/PowerDNS/pdns/commit/09bec1f): GCC 5.2 support
and sync boost.m4 macro with upstream (Kees Monshouwer among others)
- [commit 1ad4e44](https://github.com/PowerDNS/pdns/commit/1ad4e44): Log
answer packets only if log-dns-details is enabled (Kees Monshouwer)

# PowerDNS Recursor 3.6.4
Released 9th of June 2015

This is a security release fixing [Security Advisory
2015-01](security/powerdns-advisory-2015-01.md)

Bug fixes:

- [commit bccd068](https://github.com/PowerDNS/pdns/commit/bccd068): Limit the
maximum length of a qname

# PowerDNS Recursor 3.7.3
Released 9th of June 2015

Bug fixes:

- [commit 92f7b2b](https://github.com/PowerDNS/pdns/commit/92f7b2b): Limit the
maximum length of a qname

This is a security release fixing [Security Advisory
2015-01](security/powerdns-advisory-2015-01.md)

Improvements:

- [commit 46366a5](https://github.com/PowerDNS/pdns/commit/46366a5),
[commit f318a7d](https://github.com/PowerDNS/pdns/commit/f318a7d): pdnssec:
check for glue and delegations in parent zones (Kees Monshouwer)

# PowerDNS Authoritative Server 3.3.3
Released 9th of June 2015

This is a security release fixing [Security Advisory
2015-01](security/powerdns-advisory-2015-01.md)

Bug fixes:

- [commit a0a1482](https://github.com/PowerDNS/pdns/commit/a0a1482): Limit the
maximum length of a qname

# PowerDNS Authoritative Server 3.4.5
Released 9th of June 2015

This is a security release fixing [Security Advisory
2015-01](security/powerdns-advisory-2015-01.md)

Bug fixes:

- [commit ffaae2b](https://github.com/PowerDNS/pdns/commit/ffaae2b): be
careful reading empty lines in our config parser and prevent integer overflow.
- [commit 8e30209](https://github.com/PowerDNS/pdns/commit/8e30209): prevent
crash after --list-modules (Ruben Kerkhof)
- [commit 6cf71cf](https://github.com/PowerDNS/pdns/commit/6cf71cf): Limit the
maximum length of a qname

Improvements:

- [commit 28ba3fc](https://github.com/PowerDNS/pdns/commit/28ba3fc),
[commit 61b316f](https://github.com/PowerDNS/pdns/commit/61b316f): Support
/etc/default for our debian/ubuntu packages (Aki Tuomi)
- [commit d80e2b6](https://github.com/PowerDNS/pdns/commit/d80e2b6): Detect
GCC 5.1 for boost (Ruben Kerkhof)
- [commit 68b4834](https://github.com/PowerDNS/pdns/commit/68b4834),
[commit 3b14545](https://github.com/PowerDNS/pdns/commit/3b14545),
[commit 2356d5c](https://github.com/PowerDNS/pdns/commit/2356d5c),
[commit 432808b](https://github.com/PowerDNS/pdns/commit/432808b):
Various PKCS#11 fixes and improvements (Aki Tuomi)
- [commit bf357ff](https://github.com/PowerDNS/pdns/commit/bf357ff),
[commit 2433d2e](https://github.com/PowerDNS/pdns/commit/2433d2e),
[commit 8fabf4d](https://github.com/PowerDNS/pdns/commit/8fabf4d): Fix
Coverity issues (Aki Tuomi)
- [commit 5d02d01](https://github.com/PowerDNS/pdns/commit/5d02d01)
[commit 7798aa3](https://github.com/PowerDNS/pdns/commit/7798aa3),
[commit 9f6e411](https://github.com/PowerDNS/pdns/commit/9f6e411),
[commit e25a09c](https://github.com/PowerDNS/pdns/commit/e25a09c): Fix
building on OpenBSD (Florian Obser and Ruben Kerkhof)
- [commit 5c8bba2](https://github.com/PowerDNS/pdns/commit/5c8bba2): Look for
mbedtls before polarssl (Ruben Kerkhof)
- [commit 5abd150](https://github.com/PowerDNS/pdns/commit/5abd150): Let
pkg-config determine botan dependency libs (Ruben Kerkhof)
- [commit ba4d623](https://github.com/PowerDNS/pdns/commit/ba4d623): kill some
further mallocs and add note to remind us not to add them back
- [commit 50346d8](https://github.com/PowerDNS/pdns/commit/50346d8): Move
remotebackend-unix test socket to testsdir (Aki Tuomi)
- [commit 32e9512](https://github.com/PowerDNS/pdns/commit/32e9512): Defer
launch of coprocess until first question (Aki Tuomi)
- [commit d9b3ecb](https://github.com/PowerDNS/pdns/commit/d9b3ecb),
[commit 561373e](https://github.com/PowerDNS/pdns/commit/561373e): pdnssec:
check for glue and delegations in parent zones (Kees Monshouwer)

# PowerDNS Authoritative Server 3.3.2

Released 1st of May, 2015

Among other bug fixes and improvements (as listed below), this release
incorporates a fix for CVE-2015-1868, as detailed in [PowerDNS Security
Advisory 2015-01](security/powerdns-advisory-2015-01.md)

If you are running DNSSEC with version 3.3.1 or below, and you cannot
currently upgrade to 3.4.4, please consider upgrading to 3.3.2; it has a lot
of improvements and bug fixes and tremendously increases compliance.

We want to explicitly thank Kees Monshouwer for digging up all the DNSSEC
improvements and porting them back to this release.

When upgrading, please run "pdnssec rectify-all-zones" and trigger an AXFR for
all DNSSEC zones to make sure you benefit from all the compliance improvements
present in this version.

Security fixes:

- [commit 9df4944](https://github.com/PowerDNS/pdns/commit/9df4944): import CVE-2015-1868 patch (Peter van Dijk)
- [commit dbedfc5](https://github.com/PowerDNS/pdns/commit/dbedfc5): kill some further mallocs and add note to remind us not to add them back (bert hubert)

Improvements:

- [commit d0af589](https://github.com/PowerDNS/pdns/commit/d0af589)
, [commit c45b6db](https://github.com/PowerDNS/pdns/commit/c45b6db)
, [commit 88c1f21](https://github.com/PowerDNS/pdns/commit/88c1f21)
, [commit 2a4c620](https://github.com/PowerDNS/pdns/commit/2a4c620)
, [commit 4a4597e](https://github.com/PowerDNS/pdns/commit/4a4597e)
, [commit 9fa7373](https://github.com/PowerDNS/pdns/commit/9fa7373)
, [commit 8115a83](https://github.com/PowerDNS/pdns/commit/8115a83):
implement security polling for auth
- [commit 5bbd868](https://github.com/PowerDNS/pdns/commit/5bbd868): import suck() from master (Kees Monshouwer)
- [commit 194f4d2](https://github.com/PowerDNS/pdns/commit/194f4d2): respond REFUSED instead of NOERROR for "unknown zone" situations (Peter van Dijk)
- [commit 55b0653](https://github.com/PowerDNS/pdns/commit/55b0653): set AA on CNAME into referral, fixes [ticket #589](https://github.com/PowerDNS/pdns/issues/589) (Peter van Dijk)
- [commit 71232aa](https://github.com/PowerDNS/pdns/commit/71232aa): update l.root ip (Kees Monshouwer)

Bug fixes:

- [commit 88c52fe](https://github.com/PowerDNS/pdns/commit/88c52fe): make makeRelative() case insensitive (Kees Monshouwer)

DNSSEC improvements:

- [commit b3dec9c](https://github.com/PowerDNS/pdns/commit/b3dec9c): change default for add-superfluous-nsec3-for-old-bind config option (Kees Monshouwer)
- [commit 017a78b](https://github.com/PowerDNS/pdns/commit/017a78b): limit the number of NSEC3 iterations RFC5155 10.3 (Kees Monshouwer)
- [commit d768d7f](https://github.com/PowerDNS/pdns/commit/d768d7f): NSEC3 and related RRSIGS are not part of the dnstree (Kees Monshouwer)
- [commit 3a36a1c](https://github.com/PowerDNS/pdns/commit/3a36a1c): import bindbackend rectify code from master (Kees Monshouwer)
- [commit 1ee7e22](https://github.com/PowerDNS/pdns/commit/1ee7e22): limit mode 0 closest provable encloser to optout (Kees Monshouwer)
- [commit bbc0bc5](https://github.com/PowerDNS/pdns/commit/bbc0bc5): fix for errata 3441 of RFC5155 (Kees Monshouwer)
- [commit e8bfa7b](https://github.com/PowerDNS/pdns/commit/e8bfa7b): allow covering NSEC3 record in NODATA response (Kees Monshouwer)
- [commit f0b3b24](https://github.com/PowerDNS/pdns/commit/f0b3b24): return NOTIMP for direct RRSIG request (Kees Monshouwer)
- [commit c79addc](https://github.com/PowerDNS/pdns/commit/c79addc): import pdnssec checkZone() from master (Kees Monshouwer)
- [commit 2f1fec7](https://github.com/PowerDNS/pdns/commit/2f1fec7): import pdnssec rectifyZone() from master (Kees Monshouwer)

# PowerDNS Recursor 3.7.2

Released 23rd of April, 2015

Among other bug fixes and improvements (as listed below), this release incorporates a fix for 
CVE-2015-1868, as detailed in [PowerDNS Security Advisory 2015-01](security/powerdns-advisory-2015-01.md)

Bug fixes:

- [commit adb10be](https://github.com/PowerDNS/pdns/commit/adb10be) [commit 3ec3e0f](https://github.com/PowerDNS/pdns/commit/3ec3e0f) [commit dc02ebf](https://github.com/PowerDNS/pdns/commit/dc02ebf) Fix handling of forward references in label compressed packets; fixes CVE-2015-1868
- [commit a7be3f1](https://github.com/PowerDNS/pdns/commit/a7be3f1): make sure
we never call sendmsg with msg_control!=NULL && msg_controllen>0. Fixes
[ticket #2227](https://github.com/PowerDNS/pdns/issues/2227)
- [commit 9d835ed](https://github.com/PowerDNS/pdns/commit/9d835ed): Improve
robustness of root-nx-trust.

Improvements:

- [commit 99c595b](https://github.com/PowerDNS/pdns/commit/99c595b): Silence
warnings that always occur on FreeBSD (Ruben Kerkhof)

# PowerDNS Recursor 3.6.3

Released 23rd of April, 2015

The only difference between Recursor 3.6.2 and 3.6.3 is a fix for CVE-2015-1868, as detailed in [PowerDNS Security Advisory 2015-01](security/powerdns-advisory-2015-01.md)

# PowerDNS Authoritative Server 3.4.4

Released 23rd of April, 2015

**Warning**: Version 3.4.4 of the PowerDNS Authoritative Server is a major upgrade if you are coming from 2.9.x. Additionally, if you are coming from any 3.x version (including 3.3.1), there is a mandatory SQL schema upgrade. Please refer to the [Upgrade documentation](authoritative/upgrading.md) for important information on correct and stable operation, as well as notes on performance and memory use.

Among other bug fixes and improvements (as listed below), this release incorporates a fix for 
CVE-2015-1868, as detailed in [PowerDNS Security Advisory 2015-01](security/powerdns-advisory-2015-01.md)

Bug fixes:

- [commit ac3ae09](https://github.com/PowerDNS/pdns/commit/ac3ae09): fix rectify-(all)-zones for mixed case domain names
- [commit 2dea55e](https://github.com/PowerDNS/pdns/commit/2dea55e), [commit 032d565](https://github.com/PowerDNS/pdns/commit/032d565), [commit 55f2dbf](https://github.com/PowerDNS/pdns/commit/55f2dbf): fix CVE-2015-1868
- [commit 21cdbe5](https://github.com/PowerDNS/pdns/commit/21cdbe5): Blocking
IO in busy-wait for remote backend (Wieger Opmeer)
- [commit cc7b2ac](https://github.com/PowerDNS/pdns/commit/cc7b2ac): fix
double dot for root MX/SRV in bind slave zone files (Kees Monshouwer)
- [commit c40307b](https://github.com/PowerDNS/pdns/commit/c40307b): Properly
lock lmdb database, fixes [ticket #1954](https://github.com/PowerDNS/pdns/issues/1954)
(Aki Tuomi)
- [commit 662e76d](https://github.com/PowerDNS/pdns/commit/662e76d): Fix
segfault in zone2lmdb (Ruben Kerkhof)

New Features:

- [commit 5ae212e](https://github.com/PowerDNS/pdns/commit/5ae212e): pdnssec: warn for insecure wildcards in opt-out zones
- commits [cd3f21c](https://github.com/PowerDNS/pdns/commit/cd3f21c),
[8b582f6](https://github.com/PowerDNS/pdns/commit/8b582f6),
[0b7e766](https://github.com/PowerDNS/pdns/commit/0b7e766),
[f743af9](https://github.com/PowerDNS/pdns/commit/f743af9),
[dcde3c8](https://github.com/PowerDNS/pdns/commit/dcde3c8) and
[f12fcf7](https://github.com/PowerDNS/pdns/commit/f12fcf7):
TKEY record type (Aki Tuomi)
- commits [0fda1d9](https://github.com/PowerDNS/pdns/commit/0fda1d9),
[3dd139d](https://github.com/PowerDNS/pdns/commit/3dd139d),
[ba146ce](https://github.com/PowerDNS/pdns/commit/ba146ce),
[25109e2](https://github.com/PowerDNS/pdns/commit/25109e2),
[c011a01](https://github.com/PowerDNS/pdns/commit/c011a01),
[0600350](https://github.com/PowerDNS/pdns/commit/0600350),
[fc96b5e](https://github.com/PowerDNS/pdns/commit/fc96b5e),
[4414468](https://github.com/PowerDNS/pdns/commit/4414468),
[c163d41](https://github.com/PowerDNS/pdns/commit/c163d41),
[f52c7f6](https://github.com/PowerDNS/pdns/commit/f52c7f6),
[8d56a31](https://github.com/PowerDNS/pdns/commit/8d56a31),
[7821417](https://github.com/PowerDNS/pdns/commit/7821417),
[ea62bd9](https://github.com/PowerDNS/pdns/commit/ea62bd9),
[c5ababd](https://github.com/PowerDNS/pdns/commit/c5ababd),
[91c8351](https://github.com/PowerDNS/pdns/commit/91c8351) and
[073ac49](https://github.com/PowerDNS/pdns/commit/073ac49): Many
PKCS#11 improvements (Aki Tuomi)
- commits [6f0d4f1](https://github.com/PowerDNS/pdns/commit/6f0d4f1) and
[5eb33cb](https://github.com/PowerDNS/pdns/commit/5eb33cb): Introduce
xfrBlobNoSpaces and use them for TSIG (Aki Tuomi)

Improvements:

- [commit e4f48ab](https://github.com/PowerDNS/pdns/commit/e4f48ab): allow "pdnssec set-nsec3 ZONE" for insecure zones; this saves on one rectify when securing a NSEC3 zone
- commits [cce95b9](https://github.com/PowerDNS/pdns/commit/cce95b9),
[e2e9243](https://github.com/PowerDNS/pdns/commit/e2e9243) and
[e82da97](https://github.com/PowerDNS/pdns/commit/e82da97): Improvements
to the config-file parsing (Aki Tuomi)
- [commit 2180e21](https://github.com/PowerDNS/pdns/commit/2180e21):
postgresql check should not touch LDFLAGS (Ruben Kerkhof)
- [commit 0481021](https://github.com/PowerDNS/pdns/commit/0481021): Log error
when remote cannot do AXFR (Aki Tuomi)
- [commit 1ecc3a5](https://github.com/PowerDNS/pdns/commit/1ecc3a5): Speed
improvements when AXFR is disabled (Christian Hofstaedtler)
- commits [1f7334e](https://github.com/PowerDNS/pdns/commit/1f7334e) and
[b17799a](https://github.com/PowerDNS/pdns/commit/b17799a): NSEC3 and
related RRSIGS are not part of the dnstree (Kees Monshouwer)
- commits [dd943dd](https://github.com/PowerDNS/pdns/commit/dd943dd) and
[58c4834](https://github.com/PowerDNS/pdns/commit/58c4834): Change
ifdef to check for `__GLIBC__` instead of `__linux__` to prevent errors with other
libc's (James Taylor)
- [commit c929d50](https://github.com/PowerDNS/pdns/commit/c929d50): Try to
raise open files before dropping privileges (Aki Tuomi)
- [commit 69fd3dc](https://github.com/PowerDNS/pdns/commit/69fd3dc): Add
newline to carbon error message on auth (Aki Tuomi)
- [commit 3064f80](https://github.com/PowerDNS/pdns/commit/3064f80): Make sure
we send servfail on error (Aki Tuomi)
- [commit b004529](https://github.com/PowerDNS/pdns/commit/b004529): Ship
lmdb-example.pl in tarball (Ruben Kerkhof)
- [commit 9e6b24f](https://github.com/PowerDNS/pdns/commit/9e6b24f): Allocate
TCP buffer dynamically, decreasing stack usage
- [commit 267fdde](https://github.com/PowerDNS/pdns/commit/267fdde): throw if getSOA gets non-SOA record

# PowerDNS Authoritative Server 3.4.3

**Warning**: Version 3.4.3 of the PowerDNS Authoritative Server is a major upgrade if you are coming from 2.9.x. Additionally, if you are coming from any 3.x version (including 3.3.1), there is a mandatory SQL schema upgrade. Please refer to the [Upgrade documentation](authoritative/upgrading.md) for important information on correct and stable operation, as well as notes on performance and memory use.

Released March 2nd, 2015

Find the downloads [on our download page](https://www.powerdns.com/downloads.html).

Bug fixes:

- [commit ceb49ce](https://github.com/PowerDNS/pdns/commit/ceb49ce):
pdns_control: exit 1 on unknown command (Ruben Kerkhof)
- [commit 1406891](https://github.com/PowerDNS/pdns/commit/1406891): evaluate
KSK ZSK pairs per algorithm (Kees Monshouwer)
- [commit 3ca050f](https://github.com/PowerDNS/pdns/commit/3ca050f): always
set di.notified_serial in getAllDomains (Kees Monshouwer)
- [commit d9d09e1](https://github.com/PowerDNS/pdns/commit/d9d09e1):
pdns_control: don't open socket in /tmp (Ruben Kerkhof)

New features:

- [commit 2f67952](https://github.com/PowerDNS/pdns/commit/2f67952): Limit who
can send us AXFR notify queries (Ruben Kerkhof)

Improvements:

- [commit d7bec64](https://github.com/PowerDNS/pdns/commit/d7bec64): respond
REFUSED instead of NOERROR for "unknown zone" situations
- [commit ebeb9d7](https://github.com/PowerDNS/pdns/commit/ebeb9d7): Check for
Lua 5.3 (Ruben Kerkhof)
- [commit d09931d](https://github.com/PowerDNS/pdns/commit/d09931d): Check
compiler for relro support instead of linker (Ruben Kerkhof)
- [commit c4b0d0c](https://github.com/PowerDNS/pdns/commit/c4b0d0c): Replace
PacketHandler with UeberBackend where possible (Christian Hofstaedtler)
- [commit 5a85152](https://github.com/PowerDNS/pdns/commit/5a85152):
PacketHandler: Share UeberBackend with DNSSECKeeper (Christian Hofstaedtler)
- [commit 97bd444](https://github.com/PowerDNS/pdns/commit/97bd444): fix
building with GCC 5

Experimental API changes (Christian Hofstaedtler):

- [commit ca44706](https://github.com/PowerDNS/pdns/commit/ca44706): API: move
shared DomainInfo reader into it's own function
- [commit 102602f](https://github.com/PowerDNS/pdns/commit/102602f): API:
allow writing to domains.account field
- [commit d82f632](https://github.com/PowerDNS/pdns/commit/d82f632): API: read
and expose domain account field
- [commit 2b06977](https://github.com/PowerDNS/pdns/commit/2b06977): API: be
more strict when parsing record contents
- [commit 2f72b7c](https://github.com/PowerDNS/pdns/commit/2f72b7c): API:
Reject unknown types (TYPE0)
- [commit d82f632](https://github.com/PowerDNS/pdns/commit/d82f632): API: read
and expose domain account field

# PowerDNS Authoritative Server 3.4.2

**Warning**: Version 3.4.2 of the PowerDNS Authoritative Server is a major upgrade if you are coming from 2.9.x. Additionally, if you are coming from any 3.x version (including 3.3.1), there is a mandatory SQL schema upgrade. Please refer to the [Upgrade documentation](authoritative/upgrading.md) for important information on correct and stable operation, as well as notes on performance and memory use.

Released February 3rd, 2015

Find the downloads [on our download page](https://www.powerdns.com/downloads.html).

This is a performance and bugfix update to 3.4.1 and any earlier version. For high traffic setups, including
those using DNSSEC, upgrading to 3.4.2 may show tremendous performance increases.

A list of changes since 3.4.1 follows.

Improvements:

- [commit 73004f1](https://github.com/PowerDNS/pdns/commit/73004f1): implement CORS for the HTTP API
- [commit 4d9c289](https://github.com/PowerDNS/pdns/commit/4d9c289): qtype is now case insensitive in API and database
- [commit 13af5d8](https://github.com/PowerDNS/pdns/commit/13af5d8), [commit 223373a](https://github.com/PowerDNS/pdns/commit/223373a), [commit 1d5a68d](https://github.com/PowerDNS/pdns/commit/1d5a68d), [commit 705a73f](https://github.com/PowerDNS/pdns/commit/705a73f), [commit b418d52](https://github.com/PowerDNS/pdns/commit/b418d52): Allow (optional) PIE hardening
- [commit 2f86f20](https://github.com/PowerDNS/pdns/commit/2f86f20): json-api: remove priority from json
- [commit cefcf9f](https://github.com/PowerDNS/pdns/commit/cefcf9f): backport remotebackend fixes
- [commit 920f987](https://github.com/PowerDNS/pdns/commit/920f987), [commit dd8853c](https://github.com/PowerDNS/pdns/commit/dd8853c): Support Lua 5.3
- [commit 003aae5](https://github.com/PowerDNS/pdns/commit/003aae5): support single-type ZSK signing
- [commit 1c57e1d](https://github.com/PowerDNS/pdns/commit/1c57e1d): Potential fix for [ticket #1907](https://github.com/PowerDNS/pdns/issues/1907), we now try to trigger libgcc_s.so.1 to load before we chroot. I can't reproduce the bug on my local system, but this "should" help. Seriously.
- [commit 031ab21](https://github.com/PowerDNS/pdns/commit/031ab21): update polarssl to 1.3.9

Bug fixes:

- [commit 60b2b7c](https://github.com/PowerDNS/pdns/commit/60b2b7c), [commit d962fbc](https://github.com/PowerDNS/pdns/commit/d962fbc): refuse overly long labels in names
- [commit a64fd6a](https://github.com/PowerDNS/pdns/commit/a64fd6a): auth: limit long version strings to 63 characters and catch exceptions in secpoll
- [commit fa52e02](https://github.com/PowerDNS/pdns/commit/fa52e02): pdnssec: fix ttl check for RRSIG records
- [commit 0678b25](https://github.com/PowerDNS/pdns/commit/0678b25): fix up latency reporting for sub-millisecond latencies (would clip to 0)
- [commit d45c1f1](https://github.com/PowerDNS/pdns/commit/d45c1f1): make sure we don't throw an exception on "pdns_control show" of an unknown variable
- [commit 63c8088](https://github.com/PowerDNS/pdns/commit/63c8088): fix startup race condition with carbon thread already trying to broadcast uninitialized data
- [commit 796321c](https://github.com/PowerDNS/pdns/commit/796321c): make qsize-q more robust
- [commit 407867c](https://github.com/PowerDNS/pdns/commit/407867c): mind04 discovered we count corrupt packets and EAGAIN situations as validly received packets, skewing the udp questions/answers graphs on auth.
- [commit f06d069](https://github.com/PowerDNS/pdns/commit/f06d069): make latency & qsize reporting 'live'. Plus fix that we only reported the qsize of the first distributor.
- [commit 2f3498e](https://github.com/PowerDNS/pdns/commit/2f3498e): fix up statbag for carbon protocol and function pointers
- [commit 0f2f999](https://github.com/PowerDNS/pdns/commit/0f2f999): get priority from table in Lua axfrfilter; fixes [ticket #1857](https://github.com/PowerDNS/pdns/issues/1857)
- [commit 96963e2](https://github.com/PowerDNS/pdns/commit/96963e2), [commit bbcbbbe](https://github.com/PowerDNS/pdns/commit/bbcbbbe), [commit d5c9c07](https://github.com/PowerDNS/pdns/commit/d5c9c07): various backends: fix records pointing at root
- [commit e94c2c4](https://github.com/PowerDNS/pdns/commit/e94c2c4): remove additional layer of trailing . stripping, which broke MX records to the root in the BIND backend. Should close [ticket #1243](https://github.com/PowerDNS/pdns/issues/1243).
- [commit 8f35ba2](https://github.com/PowerDNS/pdns/commit/8f35ba2): api: use uncached results for getKeys()
- [commit c574336](https://github.com/PowerDNS/pdns/commit/c574336): read ALLOW-AXFR-FROM from the backend with the metadata

Minor changes:

- [commit 1e39b4c](https://github.com/PowerDNS/pdns/commit/1e39b4c): move manpages to section 1
- [commit b3992d9](https://github.com/PowerDNS/pdns/commit/b3992d9): secpoll: Replace ~ with _
- [commit 9799ef5](https://github.com/PowerDNS/pdns/commit/9799ef5): only zones with an active ksk are secure
- [commit d02744f](https://github.com/PowerDNS/pdns/commit/d02744f): api: show keys for zones without active ksk

New features:

- [commit 1b97ba0](https://github.com/PowerDNS/pdns/commit/1b97ba0): add signatures metric to auth, so we can plot signatures/second
- [commit 92cef2d](https://github.com/PowerDNS/pdns/commit/92cef2d): pdns_control: make it possible to notify all zones at once
- [commit f648752](https://github.com/PowerDNS/pdns/commit/f648752): JSON API: provide flush-cache, notify, axfr-retrieve
- [commit 02653a7](https://github.com/PowerDNS/pdns/commit/02653a7): add 'bench-db' to do very simple database backend performance benchmark
- [commit a83257a](https://github.com/PowerDNS/pdns/commit/a83257a): enable callback based metrics to statbas, and add 5 such metrics: uptime, sys-msec, user-msec, key-cache-size, meta-cache-size, signature-cache-size

Performance improvements:

- [commit a37fe8c](https://github.com/PowerDNS/pdns/commit/a37fe8c): better key for packetcache
- [commit e5217bb](https://github.com/PowerDNS/pdns/commit/e5217bb): don't do time(0) under signature cache lock
- [commit d061045](https://github.com/PowerDNS/pdns/commit/d061045), [commit 135db51](https://github.com/PowerDNS/pdns/commit/135db51), [commit 7d0f392](https://github.com/PowerDNS/pdns/commit/7d0f392): shard the packet cache, closing [ticket #1910](https://github.com/PowerDNS/pdns/issues/1910).
- [commit d71a712](https://github.com/PowerDNS/pdns/commit/d71a712): with thanks to Jack Lloyd, this works around the default Botan allocator slowing down for us during production use.


# PowerDNS Recursor 3.7.0

Unreleased, please see the 3.7.1 changelog below.

# PowerDNS Recursor 3.7.1

Released February 12th, 2015.

This version contains a mix of speedups and improvements, the combined effect of which is vastly
improved resilience against traffic spikes and malicious query overloads. 

Of further note is the massive community contribution, mostly over
Christmas.  Especially Ruben Kerkhof, Pieter Lexis, Kees Monshouwer and Aki
Tuomi delivered a lot of love.  Thanks!

Minor changes:

- Removal of dead code here and there 04dc6d618734fc630122de4c56dff641ebaf0988
- Per-qtype response counters are now 64 bit 297bb6acf7902068693a4aae1443c424d0e8dd52 on 64 bit systems
- Add IPv6 addresses for b and c.root-servers.net hints efc2595423c9a1be6f2d8f4da25445198ceb8b57
- Add IP address to logging about terminated queries 37aa9904d1cc967ba4b5d5e17dbe41485f8cdece
- Improve qtype name logging fab3ed3453e15ae88e29a0e4071b214eb19caad9 (Aki Tuomi)
- Redefine 'BAD_NETS' for dont-query based on newer IANA guidance 12cd44ee0fcde5893f85dccc499bfc35152c5fff (lochiiconnectivity)
- Add documentation links to systemd unit eb154adfdffa5c78624e2ea98e938d7b5787119e (Ruben Kerkhof)

Improvements:

- Upgrade embedded PolarSSL to 1.3.9: d330a2ea1a93d7675ef680311f8aa0306aeefcf1
- yahttp upgrade c290975778942ed1082ca66918695a5bd2d6bac4 c65a57e888ee48eaa948e590c90c51420bffa847 (Aki Tuomi)
- Replace . in hostnames by - for Carbon so as not to confuse Metronome 46541751ed1c3bc051d78217543d5fc76733e212 
- Manpages got a lot of love and are now built from Markdown (Pieter Lexis)
- Move to PolarSSL base64 488360551009784ab35c43ee4580e773a2a8a227 (Kees Monshouwer)
- The quiet=no query logging is now more informative 461df9d20c560d240285f772c09b3beb89d46daa
- We can finally bind to 0.0.0.0 and :: and guarantee answers from the correct source b71b60ee73ef3c86f80a2179981eda2e61c4363f
- We use per-packet timestamps to drop ancient traffic in case of overload b71b60ee73ef3c86f80a2179981eda2e61c4363f, non-Linux portability in d63f0d83631c41eff203d30b0b7c475a88f1db59
- Builtin webserver can be queried with the API key in the URL again c89f8cd022c4a9409b95d22ffa3b03e4e98dc400
- Ringbuffers are now available via API c89f8cd022c4a9409b95d22ffa3b03e4e98dc400
- Lua 5.3 compatibility 59c6fc3e3931ca87d484337daee512e716bc4cf4 (Kees Monshouwer)
- No longer leave a stale UNIX domain socket around from rec_control if the recursor was down 524e4f4d81f4ed9eb218715cbc8a59f0b9868234, 
  ticket #2061
- Running with 'quiet=no' would strangely actually prevent debug messages from being logged f48d7b657ec32517f8bfcada3bfe6353ca313314
- Webserver now implements CORS for the API ea89a97e864c43c1cb03f2959ad04c4ebe7580ad, fixing ticket #1984
- Houskeeping thread would sometimes run multiple times simultaneously, which worked, but was odd cc59bce675e62e2b9657b42614ce8be3312cae82

New features:

- New `root-nx-trust` flag makes PowerDNS generalize NXDOMAIN responses from the root-servers 01402d56846a3a61811ebd4e6bc97e53f908e568
- `getregisteredname()` for Lua, which turns 'www.bbc.co.uk' into 'bbc.co.uk' 8cd4851beb78bc6ab320926fb5cb6a09282016b1
- Lua preoutquery filter 3457a2a0ec41d3b3aff7640f30008788e1228a6e
- Lua IP-based filter (ipfilter) before parsing packets 4ea949413c495254acb0bd19335142761c1efc0c
- `iputils` class for Lua, to quickly process IP addresses and netmasks in their native format
- `getregisteredname` function for Lua, to find the registered domain for a given name
- Various new ringbuffers: top-servfail-remotes, top-largeanswer-remotes, top-servfail-queries

Speedups:

- Remove unneeded malloc traffic 93d4a89096e64d53740790f58fadec56f6a0af14 8682c32bc45b6ffa7c0f6da778e1b223ae7f03ce a903b39cfe7364c56324038264d3db50b8cece87
- Our nameserver-loop detection carried around a lot of baggage for complex domain names, plus did not differentiate IPv4 and IPv6 well enough 891fbf888ccac074e3edc38864641ca774f2f03c
- Prioritize new queries over nameserver responses, improving latency under query bursts bf3b0cec366c090af000b066267b6f6bbb3a512a
- Remove escaping in case there was nothing to escape 83b746fd1d94c8742d8bd87a44beb44c154230c7
- Our logging infrastructure had a lot of locking d1449e4d073595e1e1581804f121fc90e37158bf
- Reduce logging level of certain common messages, which locked up synchronously logging systems 854d44e31c76aa650520e6d462dd3a02b5936f7a
- Add limit on total wall-clock time spent on a query 9de3e0340fa066d4c59449e1643a1de8c343f8f2
- Packet cache is now case-insensitive, which increases hitrate 90974597aadaf1096e3fd0dc450be7422ea591a5

Security relevant:

- Check for PIE, RELRO and stack protector during configure 8d0354b189c12e1e14f5309d3b49935c17f9eeb0 (Aki Tuomi)
- Testing for support of PIE etc was improved in b2053c28ccb9609e2ce7bcb6beda83f98a062aa3 and beyond, fixes #2125 (Ruben Kerkhof)
- Max query-per-query limit (max-qperq) is now configurable 173d790ead08f67733010ca4c6fc404a040fe699

Bugs fixed:

- IPv6 outgoing queries had a disproportionate effect on our query load. Fixed in 76f190f2a0877cd79ede2994124c1a58dc69ae49 and beyond.
- rec_control gave incorrect output on a timeout 12997e9d800734da51b808767e1e2477244c30eb
- When using the webserver AND having an error in the Lua script, recursor could crash during startup 62f0ae62984adadab687c23fe1b287c1f219b2cb
- Hugely long version strings would trip up security polling 18b7333828a1275ae5f5574a9c8330290d8557ff (Kees Monshouwer)
- The 'remotes' ringbuffer was sized incorrectly f8f243b01215d6adcb59389f09ef494f1309041f
- Cache sizes had an off-by-one scaling problem, with the wrong number of entries allocated per thread f8f243b01215d6adcb59389f09ef494f1309041f
- Our automatic file descriptor limit raising was attempted *after* setuid, which made it a lot less effective. Found and fixed by Aki Tuomi
  a6414fdce9b0ec32c340d1f2eea2254f3fedc1c1
- Timestamps used for dropping packets were occasionally wrong 183eb8774e4bc2569f06d5894fec65740f4b70b6 and 4c4765c104bacc146533217bcc843efb244a8086 
  (RC2) with thanks to Winfried for debugging.
- In RC1, our new DoS protection measures would crash the Recursor if too many root servers were unreachable.
  6a6fb05ad81c519b4002ed1db00f3ed9b7bce6b4. Debugging and testing by Fusl.

 
Various other documentation changes by Christian Hofstaedtler and Ruben
Kerkhof.  Lots of improvements all over the place by Kees Monshouwer.

# PowerDNS Recursor 3.6.2

**Note**: Version 3.6.2 is a bugfix update to 3.6.1. Released on the 30th of October 2014.

[Official download page](https://www.powerdns.com/downloads.html)

A list of changes since 3.6.1 follows.

-   [commit ab14b4f](https://github.com/PowerDNS/pdns/commit/ab14b4f): expedite servfail generation for ezdns-like failures (fully abort query resolving if we hit more than 50 outqueries). This also prevents the issue documented in [PowerDNS Security Advisory 2014-02](security/powerdns-advisory-2014-02/) (CVE-2014-8601)
-   [commit 42025be](https://github.com/PowerDNS/pdns/commit/42025be): PowerDNS now polls the security status of a release at startup and periodically. More detail on this feature, and how to turn it off, can be found in [Security polling](common/security.md#security-polling).
-   [commit 5027429](https://github.com/PowerDNS/pdns/commit/5027429): We did not transmit the right 'local' socket address to Lua for TCP/IP queries in the recursor. In addition, we would attempt to lookup a filedescriptor that wasn't there in an unlocked map which could conceivably lead to crashes. Closes [ticket 1828](https://github.com/PowerDNS/pdns/issues/1828), thanks Winfried for reporting
-   [commit 752756c](https://github.com/PowerDNS/pdns/commit/752756c): Sync embedded yahttp copy. API: Replace HTTP Basic auth with static key in custom header
-   [commit 6fdd40d](https://github.com/PowerDNS/pdns/commit/6fdd40d): add missing `#include <pthread.h>` to rec-channel.hh (this fixes building on OS X).

# PowerDNS Authoritative Server 3.4.1

**Warning**: Version 3.4.1 of the PowerDNS Authoritative Server is a major upgrade if you are coming from 2.9.x. Additionally, if you are coming from any 3.x version (including 3.3.1), there is a mandatory SQL schema upgrade. Please refer to the [Upgrade documentation](authoritative/upgrading.md) for important information on correct and stable operation, as well as notes on performance and memory use.

Released October 30th, 2014

Find the downloads [on our download page](https://www.powerdns.com/downloads.html).

This is a bugfix update to 3.4.0 and any earlier version.

A list of changes since 3.4.0 follows.

-   [commit dcd6524](https://github.com/PowerDNS/pdns/commit/dcd6524), [commit a8750a5](https://github.com/PowerDNS/pdns/commit/a8750a5), [commit 7dc86bf](https://github.com/PowerDNS/pdns/commit/7dc86bf), [commit 2fda71f](https://github.com/PowerDNS/pdns/commit/2fda71f): PowerDNS now polls the security status of a release at startup and periodically. More detail on this feature, and how to turn it off, can be found in [Security polling](common/security.md#security-polling).
-   [commit 5fe6dc0](https://github.com/PowerDNS/pdns/commit/5fe6dc0): API: Replace HTTP Basic auth with static key in custom header (X-API-Key)
-   [commit 4a95ab4](https://github.com/PowerDNS/pdns/commit/4a95ab4): Use transaction for pdnssec increase-serial
-   [commit 6e82a23](https://github.com/PowerDNS/pdns/commit/6e82a23): Don't empty ordername during pdnssec increase-serial
-   [commit 535f4e3](https://github.com/PowerDNS/pdns/commit/535f4e3): honor SOA-EDIT while considering "empty IXFR" fallback, fixes [ticket 1835](https://github.com/PowerDNS/pdns/issues/1835). This fixes slaving of signed zones to IXFR-aware slaves like NSD or BIND.

# PowerDNS Authoritative Server 3.4.0
Released September 30th, 2014

This is a performance, feature, bugfix and conformity update to 3.3.1 and any earlier version. It contains a huge amount of work by various contributors, to whom we are very grateful.

**Warning**: Version 3.4.0 of the PowerDNS Authoritative Server is a major upgrade if you are coming from 2.9.x. Additionally, if you are coming from any 3.x version (including 3.3.1), there is a mandatory SQL schema upgrade. Please refer to the [Upgrade documentation](authoritative/upgrading.md) for important information on correct and stable operation, as well as notes on performance and memory use.

## Downloads
Find the downloads [on our download page](https://www.powerdns.com/downloads.html).

A list of changes since 3.3.1 follows.

Changes between RC2 and 3.4.0:

-   [commit ad189c9](https://github.com/PowerDNS/pdns/commit/ad189c9), [commit 445d93c](https://github.com/PowerDNS/pdns/commit/445d93c): also distribute the dnsdist manual page
-   [commit b5a276d](https://github.com/PowerDNS/pdns/commit/b5a276d), [commit 0b346e9](https://github.com/PowerDNS/pdns/commit/0b346e9), [commit 74caf87](https://github.com/PowerDNS/pdns/commit/74caf87), [commit 642fd2e](https://github.com/PowerDNS/pdns/commit/642fd2e): Make sure all backends actually work as dynamic modules
-   [commit 14b11c4](https://github.com/PowerDNS/pdns/commit/14b11c4): raise log level on dlerror(), fixes [ticket 1734](https://github.com/PowerDNS/pdns/issues/1734), thanks @James-TR
-   [commit 016d810](https://github.com/PowerDNS/pdns/commit/016d810): improve postgresql detection during ./configure
-   [commit dce1e90](https://github.com/PowerDNS/pdns/commit/dce1e90): DNAME: don't sign the synthesised CNAME
-   [commit 25e7af3](https://github.com/PowerDNS/pdns/commit/25e7af3): send empty SERVFAIL after a backend throws a DBException, instead of including useless content

Changes between RC1 and RC2:

-   [commit bb6e54f](https://github.com/PowerDNS/pdns/commit/bb6e54f): document udp6-queries, udp4-queries, add rd-queries, recursion-unanswered metrics & document. Closes [ticket 1400](https://github.com/PowerDNS/pdns/issues/1400).
-   [commit 4a23af7](https://github.com/PowerDNS/pdns/commit/4a23af7): init script: support DAEMON\_ARGS; [commit 7e5b3a0](https://github.com/PowerDNS/pdns/commit/7e5b3a0): init script: ensure socket dir exists
-   [commit dd930ed](https://github.com/PowerDNS/pdns/commit/dd930ed): don't import supermaster ips from other accounts
-   [commit ed3afdf](https://github.com/PowerDNS/pdns/commit/ed3afdf): fall back to central bind if reuseport bind fails; improves [ticket 1715](https://github.com/PowerDNS/pdns/issues/1715)
-   [commit 709ca59](https://github.com/PowerDNS/pdns/commit/709ca59): GeoIP backend implementation. This is a new backend, still experimental!
-   [commit bf5a484](https://github.com/PowerDNS/pdns/commit/bf5a484): support EVERY future version of OS X, fixes [ticket 1702](https://github.com/PowerDNS/pdns/issues/1702)
-   [commit 4dbaec6](https://github.com/PowerDNS/pdns/commit/4dbaec6): Check for \_\_FreeBSD\_kernel\_\_ as per https://lists.debian.org/debian-bsd/2006/03/msg00127.html, fixes [ticket 1684](https://github.com/PowerDNS/pdns/issues/1684); [commit 74f389d](https://github.com/PowerDNS/pdns/commit/74f389d): \_\_FreeBSD\_kernel\_\_ is defined but empty on systems with FreeBSD kernels, breaking compile. Thanks pawal
-   [commit 2e6bbd8](https://github.com/PowerDNS/pdns/commit/2e6bbd8): Catch PDNSException in Signingpiper::helperWorker to avoid abort
-   [commit 0ffd51d](https://github.com/PowerDNS/pdns/commit/0ffd51d): improve error reporting on malformed labels
-   [commit c48dec7](https://github.com/PowerDNS/pdns/commit/c48dec7): Fix forwarded TSIG message issue
-   [commit dad70f2](https://github.com/PowerDNS/pdns/commit/dad70f2): skip TCP\_DEFER\_ACCEPT on platforms that do not have it (like FreeBSD); fixes [ticket 1658](https://github.com/PowerDNS/pdns/issues/1658)
-   [commit c7287b6](https://github.com/PowerDNS/pdns/commit/c7287b6): should fix [ticket 1662](https://github.com/PowerDNS/pdns/issues/1662), reloading while checking for domains that need to be notified in BIND, causing lock
-   [commit 3e67ea8](https://github.com/PowerDNS/pdns/commit/3e67ea8): allow OPT pseudo record type in IXFR query
-   [commit a1caa8b](https://github.com/PowerDNS/pdns/commit/a1caa8b): webserver: htmlescape VERSION and config name
-   [commit df9d980](https://github.com/PowerDNS/pdns/commit/df9d980): Remove "log-failed-updates" leftover
-   [commit a1fe72a](https://github.com/PowerDNS/pdns/commit/a1fe72a): Remove unused "soa-serial-offset" option

Changes between 3.3.1 and 3.4.0-RC1 follow.

## DNSSEC changes
-   [commit bba8413](https://github.com/PowerDNS/pdns/commit/bba8413): add option (max-signature-cache-entries) to limit the maximum number of cached signatures.
-   [commit 28b66a9](https://github.com/PowerDNS/pdns/commit/28b66a9): limit the number of NSEC3 iterations (see RFC5155 10.3), with the max-nsec3-iterations option.
-   [commit b50efd6](https://github.com/PowerDNS/pdns/commit/b50efd6): drop the 'superfluous NSEC3' option that old BIND validators need.
-   The bindbackend 'hybrid' mode was reintroduced by Kees Monshouwer. Enable it with bind-hybrid.
-   Aki Tuomi contributed experimental PKCS\#11 support for DNSSEC key management with a (Soft)HSM.
-   Direct RRSIG queries now return NOTIMP.
-   [commit fa37777](https://github.com/PowerDNS/pdns/commit/fa37777): add secure-all-zones command to pdnssec
-   Unrectified zones can now get rectified 'on the fly' during outgoing AXFR. This makes it possible to run a hidden signing master without rectification.
-   [commit 82fb538](https://github.com/PowerDNS/pdns/commit/82fb538): AXFR in: don't accept zones with a mixture of Opt-Out NSEC3 RRs and non-Opt-Out NSEC3 RRs
-   Various minor bugfixes, mostly from the unstoppable Kees Monshouwer.
-   [commit 0c4c552](https://github.com/PowerDNS/pdns/commit/0c4c552): set non-zero exit status in pdnssec if an exception was thrown, for easier automatic usage.
-   [commit b8bd119](https://github.com/PowerDNS/pdns/commit/b8bd119): pdnssec -v show-zone: Print all keys instead of just entry point keys.
-   [commit 52e0d78](https://github.com/PowerDNS/pdns/commit/52e0d78): answer direct NSEC queries without DO bit
-   [commit ca2eb01](https://github.com/PowerDNS/pdns/commit/ca2eb01): output ZSK DNSKEY records if experimental-direct-dnskey support is enabled
-   [commit 83609e2](https://github.com/PowerDNS/pdns/commit/83609e2): SOA-EDIT: fix INCEPTION-INCREMENT handling
-   [commit ac4a2f1](https://github.com/PowerDNS/pdns/commit/ac4a2f1): AXFR-out can handle secure and insecure NSEC3 optout delegations
-   [commit ff47302](https://github.com/PowerDNS/pdns/commit/ff47302): AXFR-in can handle secure and insecure NSEC3 optout delegations

## New features
-   DNAME support. Enable with experimental-dname-processing.
-   PowerDNS can now send stats directly to Carbon servers. Enable with carbon-server, tweak with carbon-ourname and carbon-interval.
-   [commit 767da1a](https://github.com/PowerDNS/pdns/commit/767da1a): Add list-zone capability to pdns\_control
-   [commit 51f6bca](https://github.com/PowerDNS/pdns/commit/51f6bca): Add delete-zone to pdnssec.
-   The gsql backends now support record comments, and disabling records.
-   The new reuseport config option allows setting SO\_REUSEPORT, which allows for some performance improvements.
-   local-address-nonexist-fail and local-ipv6-nonexist-fail allow pdns to start up even if some addresses fail to bind.
-   'AXFR-SOURCE' in domainmetadata sets the source address for an AXFR retrieval.
-   [commit 451ba51](https://github.com/PowerDNS/pdns/commit/451ba51): Implement pdnssec get-meta/set-meta
-   Experimental RFC2136/DNS UPDATE support from Ruben d'Arco, with extensive testing by Kees Monshouwer.
-   pdns\_control bind-add-zone
-   New option bind-ignore-broken-records ignores out-of-zone records while loading zone files.
-   pdnssec now has commands for TSIG key management.
-   We now support other algorithms than MD5 for TSIG.
-   [commit ba7244a](https://github.com/PowerDNS/pdns/commit/ba7244a): implement pdns\_control qtypes
-   Support for += syntax for options

## Bugfixes
-   We verify the algorithm used for TSIG queries, and use the right algorithm in signing if there is possible confusion. Plus a few minor TSIG-related fixes.
-   [commit ff99a74](https://github.com/PowerDNS/pdns/commit/ff99a74): making *-threads settings empty now yields a default of one instead of zero.
-   [commit 9215e60](https://github.com/PowerDNS/pdns/commit/9215e60): we had a deadly embrace in getUpdatedMasters in bindbackend reimplementation, thanks to Winfried for detailed debugging!
-   [commit 9245fd9](https://github.com/PowerDNS/pdns/commit/9245fd9): don't addSuckRequest after supermaster zone creation to avoid one cause of simultaneous AXFR for the same zone
-   [commit 719f902](https://github.com/PowerDNS/pdns/commit/719f902): fix dual-stack superslave when multiple namservers share a ip
-   [commit 33966bf](https://github.com/PowerDNS/pdns/commit/33966bf): avoid address truncation in doNotifications
-   [commit eac85b1](https://github.com/PowerDNS/pdns/commit/eac85b1): prevent duplicate slave notifications caused by different ipv6 address formatting
-   [commit 3c8a711](https://github.com/PowerDNS/pdns/commit/3c8a711): make notification queue ipv6 compatible
-   [commit 0c13e45](https://github.com/PowerDNS/pdns/commit/0c13e45): make isMaster ip check more tolerant for different ipv6 notations
-   Various fixes for possible issues reported by Coverity Scan ([commit f17c93b](https://github.com/PowerDNS/pdns/commit/f17c93b), )
-   [commit 9083987](https://github.com/PowerDNS/pdns/commit/9083987): don't rely on included polarssl header files when using system polarssl. Spotted by Oden Eriksson of Mandriva, thanks!
-   Various users reported pdns\_control hangs, especially when using the guardian. We are confident that all causes of these hangs are now gone.
-   Decreasing the webserver ringbuffer size could cause crashes.
-   [commit 4c89cce](https://github.com/PowerDNS/pdns/commit/4c89cce): nproxy: Add missing chdir("/") after chroot()
-   [commit 016a0ab](https://github.com/PowerDNS/pdns/commit/016a0ab): actually notice timeout during AXFR retrieve, thanks hkraal

## REST API changes
-   The REST API was much improved and is nearing stability, thanks to Christian Hofstaedtler and others.
-   Mark Schouten at Tuxis contributed a zone importer.

## Other changes
-   Our tarballs and packages now include *.sql schema files for the SQL backends.
-   The webserver (including API) now has an ACL (webserver-allow-from).
-   Webserver (including API) is now powered by YaHTTP.
-   Various autotools usage improvements from Ruben Kerkhof.
-   The dist tarball is now bzip2-compressed instead of gzip.
-   Various remotebackend updates, including replacing curl with (included) yahttp.
-   Dynamic module loading is now allowed on Mac OS X.
-   The AXFR ACL (allow-axfr-ips) now defaults to 127.0.0.0/8,::1 instead of the whole world.
-   [commit ba91c2f](https://github.com/PowerDNS/pdns/commit/ba91c2f): remove unused gpgsql-socket option and document postgres socket usage
-   Improved support for Lua 5.2.
-   The edns-subnet option code is now fixed at 8, and the edns-subnet-option-numbers option has been removed.
-   geobackend now has very limited edns-subnet support - it will use the 'real' remote if available.
-   pipebackend ABI v4 adds the zone name to the AXFR command.
-   We now [avoid getaddrinfo()](http://blog.powerdns.com/2014/05/21/a-surprising-discovery-on-converting-ipv6-addresses-we-no-longer-prefer-getaddrinfo/) as much as possible.
-   The packet cache now handles (forwarded) recursive answers better, including TTL aging and respecting allow-recursion.
-   [commit ff5ba4f](https://github.com/PowerDNS/pdns/commit/ff5ba4f): pdns\_server --help no longer exits with 1.
-   Mark Zealey contributed an experimental LMDB backend. Kees Monshouwer added experimental DNSSEC support to it. Thanks, both!
-   [commit 81859ba](https://github.com/PowerDNS/pdns/commit/81859ba): No longer attempt to answer questions coming in from port 0, reply would not reach them anyhow. Thanks to Niels Bakker and sid3windr for insight & debugging. Closes [ticket 844](https://github.com/PowerDNS/pdns/issues/844).
-   RCodes are now reported in text in various places, thanks Aki.
-   Kees Monshouwer set up automatic testing for the oracle and goracle backends, and fixed various issues in them.
-   Leftovers of previous support for Windows have been removed, thanks to Kees Monshouwer, Aki Tuomi.
-   Bundled PolarSSL has been upgraded to 1.3.2
-   PolarSSL replaced previously bundled implementations of AES ([commit e22d9b4](https://github.com/PowerDNS/pdns/commit/e22d9b4)) and SHA ([commit 9101035](https://github.com/PowerDNS/pdns/commit/9101035))
-   bindbackend is now a module
-   [commit 14a2e52](https://github.com/PowerDNS/pdns/commit/14a2e52): Use the inet data type for supermasters.ip on postgresql.
-   We now send an empty SERVFAIL when a CNAME chain is too long, instead of including the partial chain.
-   [commit 3613a51](https://github.com/PowerDNS/pdns/commit/3613a51): Show built-in features in --version output
-   [commit 4bd7d35](https://github.com/PowerDNS/pdns/commit/4bd7d35): make domainmetadata queries case insensitive
-   [commit 088c334](https://github.com/PowerDNS/pdns/commit/088c334): output warning message when no to be notified NS's are found
-   [commit 5631b44](https://github.com/PowerDNS/pdns/commit/5631b44): gpsqlbackend: use empty defaults for dbname and user; libpq will use the current user name for both by default
-   [commit d87ded3](https://github.com/PowerDNS/pdns/commit/d87ded3): implement udp-truncation-threshold to override the previous 1680 byte maximum response datagram size - no matter what EDNS0 said. Plus document it.
-   Implement udp-truncation-threshold to override the previous 1680 byte maximum response datagram size - no matter what EDNS0 said.
-   Removed settings related to fancy records, as we haven't supported those since version 3.0
-   Based on earlier work by Mark Zealey, Kees Monshouwer increased our packet cache performance between 200% and 500% depending on the situation, by simplifying some code in [commit 801812e](https://github.com/PowerDNS/pdns/commit/801812e) and [commit 8403ade](https://github.com/PowerDNS/pdns/commit/8403ade).

# PowerDNS Recursor 3.6.1
**Warning**: Version 3.6.1 is a mandatory security upgrade to 3.6.0! Released on the 10th of September 2014.

PowerDNS Recursor 3.6.0 could crash with a specific sequence of packets. For more details, see [the advisory](security/powerdns-advisory-2014-01.md). PowerDNS Recursor 3.6.1 was very well tested, and is in full production already, so it should be a safe upgrade.

## Downloads
-   [Official download page](https://www.powerdns.com/downloads.html)

In addition to various fixes related to this potential crash, 3.6.1 fixes a few minor issues and adds a debugging feature:

-   We could not encode IPv6 AAAA records that mapped to IPv4 addresses in some cases (:ffff.1.2.3.4). Fixed in [commit c90fcbd](https://github.com/PowerDNS/pdns/commit/c90fcbd) , closing [ticket 1663](https://github.com/PowerDNS/pdns/issues/1663).
-   Improve systemd startup timing with respect to network availability ([commit cf86c6a](https://github.com/PowerDNS/pdns/commit/cf86c6a)), thanks to Morten Stevens.
-   Realtime telemetry can now be enabled at runtime, for example with 'rec\_control carbon-server 82.94.213.34 ourname1234'. This ties in to our existing carbon-server and carbon-ourname settings, but now at runtime. This specific invocation will make your stats appear automatically on our [public telemetry server](http://xs.powerdns.com/metronome/?server=pdns.xs.recursor&beginTime=-3600).

# PowerDNS Recursor version 3.6.0
This is a performance, feature and bugfix update to 3.5/3.5.3. It contains important fixes for slightly broken domain names, which your users expect to work anyhow. It also brings robust resilience against certain classes of attacks.

## Downloads
-   [Official download page](https://www.powerdns.com/downloads.html)
-   [native RHEL5/6 packages from Kees Monshouwer](https://www.monshouwer.eu/download/3rd_party/pdns-recursor/)

## Changes between RC1 and release
-   [commit 30b13ef](https://github.com/PowerDNS/pdns/commit/30b13ef): do not apply some of our filters to root and gtlds, plus remove some useless {}
-   [commit cc81d90](https://github.com/PowerDNS/pdns/commit/cc81d90): fix yahttp copy in dist-recursor for BSD cp
-   [commit b798618](https://github.com/PowerDNS/pdns/commit/b798618): define \_\_APPLE\_USE\_RFC\_3542 during recursor build on Darwin, fixes [ticket 1449](https://github.com/PowerDNS/pdns/issues/1449)
-   [commit 1d7f863](https://github.com/PowerDNS/pdns/commit/1d7f863): Merge pull request [ticket 1443](https://github.com/PowerDNS/pdns/issues/1443) from zeha/recursor-nostrip
-   [commit 5cdeede](https://github.com/PowerDNS/pdns/commit/5cdeede): remove (non-working) [aaaa-]additional-processing flags from the recursor. Closes [ticket 1448](https://github.com/PowerDNS/pdns/issues/1448)
-   [commit 984d747](https://github.com/PowerDNS/pdns/commit/984d747): Support building recursor on kFreeBSD and Hurd
-   [commit 79240f1](https://github.com/PowerDNS/pdns/commit/79240f1): Allow not stripping of binaries in recursor's make install
-   [commit e9c2ad3](https://github.com/PowerDNS/pdns/commit/e9c2ad3): document pdns.DROP for recursor, add policy-drops metric for it

## New features
-   [commit aadceba](https://github.com/PowerDNS/pdns/commit/aadceba): Implement minimum-ttl-override config setting, plus runtime configurability via 'rec\_control set-minimum-ttl'.
-   Lots of work on the JSON API, which is exposed via Aki Tuomi's 'yahttp'. Massive thanks to Christian Hofstaedtler for delivering this exciting new functionality. Documentation & demo forthcoming, but code to use it is available [on GitHub](https://github.com/powerdns/pdnscontrol).
-   Lua modules can now use 'pdnslog(INFO..'), as described in [ticket 1074](https://github.com/PowerDNS/pdns/issues/1074), implemented in [commit 674a305](https://github.com/PowerDNS/pdns/commit/674a305)
-   Adopt any-to-tcp feature to the recursor. Based on a patch by Winfried Angele. Closes [ticket 836](https://github.com/PowerDNS/pdns/issues/836), [commit 56b4d21](https://github.com/PowerDNS/pdns/commit/56b4d21) and [commit e661a20](https://github.com/PowerDNS/pdns/commit/e661a20).
-   [commit 2c78bd5](https://github.com/PowerDNS/pdns/commit/2c78bd5): implement built-in statistics dumper using the 'carbon' protocol, which is also understood by metronome (our mini-graphite). Use 'carbon-server', 'carbon-ourname' and 'carbon-interval' settings.
-   New setting 'udp-truncation-threshold' to configure from how many bytes we should truncate. [commit a09a8ce](https://github.com/PowerDNS/pdns/commit/a09a8ce).
-   Proper support for CHaos class for CHAOS TXT queries. [commit c86e1f2](https://github.com/PowerDNS/pdns/commit/c86e1f2), addition for lua in [commit f94c53d](https://github.com/PowerDNS/pdns/commit/f94c53d), some warnings in [commit 438db54](https://github.com/PowerDNS/pdns/commit/438db54) however.
-   Added support for Lua scripts to drop queries w/o further processing. [commit 0478c54](https://github.com/PowerDNS/pdns/commit/0478c54).
-   Kevin Holly added qtype statistics to recursor and rec\_control (get-qtypelist) ([commit 79332bf](https://github.com/PowerDNS/pdns/commit/79332bf))
-   Add support for include-files in configuration, also reload ACLs and zones defined in them ([commit 829849d](https://github.com/PowerDNS/pdns/commit/829849d), [commit 242b90e](https://github.com/PowerDNS/pdns/commit/242b90e), [commit 302df81](https://github.com/PowerDNS/pdns/commit/302df81)).
-   Paulo Anes contributed server-down-max-fails which helps combat Recursive DNS based amplification attacks. Described in [this post](http://blog.powerdns.com/2014/04/03/further-dos-guidance-packages-and-patches-available/). Also comes with new metric 'failed-host-entries' in [commit 406f46f](https://github.com/PowerDNS/pdns/commit/406f46f).
-   [commit 21e7976](https://github.com/PowerDNS/pdns/commit/21e7976): Implement "followCNAMERecords" feature in the Lua hooks.

## Improvements
-   [commit 06ea901](https://github.com/PowerDNS/pdns/commit/06ea901): make pdns-distributes-queries use a hash so related queries get sent to the same thread. Original idea by Winfried Angele. Astoundingly effective, approximately halves CPU usage!
-   [commit b13e737](https://github.com/PowerDNS/pdns/commit/b13e737): --help now writes to stdout instead of stderr. Thanks Winfried Angele.
-   To aid in limiting DoS attacks, when truncating a response, we actually truncate all the way so only the question remains. Suggested in [ticket 1092](https://github.com/PowerDNS/pdns/issues/1092), code in [commit add935a](https://github.com/PowerDNS/pdns/commit/add935a).
-   No longer experimental, the switch 'pdns-distributes-queries' can improve multi-threaded performance on Linux (various cleanup commits).
-   Update to embedded PolarSSL, plus remove previous AES implementation and shift to PolarSSL ([commit e22d9b4](https://github.com/PowerDNS/pdns/commit/e22d9b4), [commit 990ad9a](https://github.com/PowerDNS/pdns/commit/990ad9a))
-   [commit 92c0733](https://github.com/PowerDNS/pdns/commit/92c0733) moves various Lua magic constants into an enum namespace.
-   set group and supplementary groups before chroot ([commit 6ee50ce](https://github.com/PowerDNS/pdns/commit/6ee50ce), [ticket 1198](https://github.com/PowerDNS/pdns/issues/1198)).
-   [commit 4e9a20e](https://github.com/PowerDNS/pdns/commit/4e9a20e): raise our socket buffer setting so it no longer generates a warning about lowering it.
-   [commit 4e9a20e](https://github.com/PowerDNS/pdns/commit/4e9a20e): warn about Linux suboptimal IPv6 settings if we detect them.
-   SIGUSR2 turns on a 'trace' of all DNS traffic, a second SIGUSR2 now turns it off again. [commit 4f217ce](https://github.com/PowerDNS/pdns/commit/4f217ce).
-   Various fixes for Lua 5.2.
-   [commit 81859ba](https://github.com/PowerDNS/pdns/commit/81859ba): No longer attempt to answer questions coming in from port 0, reply would not reach them anyhow. Thanks to Niels Bakker and 'sid3windr' for insight & debugging. Closes [ticket 844](https://github.com/PowerDNS/pdns/issues/844).
-   [commit b1a2d6c](https://github.com/PowerDNS/pdns/commit/b1a2d6c): now, I'm not one to get OCD over things, but that log message about stats based on 1801 seconds got to me. 1800 now.

## Fixes
-   0c9de4fc: stay away from getaddrinfo unless we really can't help it for ascii ipv6 conversions to binary
-   [commit 08f3f63](https://github.com/PowerDNS/pdns/commit/08f3f63): fix average latency calculation, closing [ticket 424](https://github.com/PowerDNS/pdns/issues/424).
-   [commit 75ba907](https://github.com/PowerDNS/pdns/commit/75ba907): Some of our counters were still 32 bits, now 64.
-   [commit 2f22827](https://github.com/PowerDNS/pdns/commit/2f22827): Fix statistics and stability when running with pdns-distributes-queries.
-   [commit 6196f90](https://github.com/PowerDNS/pdns/commit/6196f90): avoid merging old and new additional data, fixes an issue caused by weird (but probably legal) Akamai behaviour
-   [commit 3a8a4d6](https://github.com/PowerDNS/pdns/commit/3a8a4d6): make sure we don't exceed the number of available filedescriptors for mthreads. Raises performance in case of DoS. See [this post](http://blog.powerdns.com/2014/02/06/related-to-recent-dos-attacks-recursor-configuration-file-guidance/) for further details.
-   [commit 7313fe6](https://github.com/PowerDNS/pdns/commit/7313fe6): implement indexed packet cache wiping for recursor, orders of magnitude faster. Important when reloading all zones, which causes massive cache cleaning.
-   rec\_control get-all would include 'cache-bytes' and 'packetcache-bytes', which were expensive operations, too expensive for frequent polling. Removed in [commit 8e42d27](https://github.com/PowerDNS/pdns/commit/8e42d27).
-   All old workarounds for supporting Windows of the XP era have been removed.
-   Fix issues on S390X based systems which have unsigned characters ([commit 916a0fd](https://github.com/PowerDNS/pdns/commit/916a0fd))

# PowerDNS Authoritative Server version 3.3.1
Released December 17th, 2013

This is a bugfix update to 3.3.

## Downloads
-   [Official download page](http://www.powerdns.com/content/downloads.html)
-   [native RHEL5/6 packages from Kees Monshouwer](http://www.monshouwer.eu/download/3rd_party/pdns-server/)

## Changes since 3.3
-   direct-dnskey is no longer experimental, thanks Kees Monshouwer & co for extensive testing ([commit e4b36a4](https://github.com/PowerDNS/pdns/commit/e4b36a4)).
-   Handle signals during poll ([commit 5dde2c6](https://github.com/PowerDNS/pdns/commit/5dde2c6)).
-   [commit 7538e56](https://github.com/PowerDNS/pdns/commit/7538e56): Fix zone2{sql,json} exit codes
-   [commit 7593c40](https://github.com/PowerDNS/pdns/commit/7593c40): geobackend: fix possible nullptr deref
-   [commit 3506cc6](https://github.com/PowerDNS/pdns/commit/3506cc6): gpsqlbackend: don't append empty dbname=/user= values to connect string
-   gpgsql queries were simplified through the use of casting ([commit 9a6e39c](https://github.com/PowerDNS/pdns/commit/9a6e39c)).
-   [commit a7aa9be](https://github.com/PowerDNS/pdns/commit/a7aa9be): Replace hardcoded make with variable
-   [commit e4fe901](https://github.com/PowerDNS/pdns/commit/e4fe901): make sure to run PKG\_PROG\_PKG\_CONFIG before the first PKG\_* usage
-   [commit 29bf169](https://github.com/PowerDNS/pdns/commit/29bf169): fix hmac-md5 TSIG key lookup
-   [commit c4e348b](https://github.com/PowerDNS/pdns/commit/c4e348b): fix 64+ character TSIG keys
-   [commit 00a7b25](https://github.com/PowerDNS/pdns/commit/00a7b25): Fix comparison between signed and unsigned by using uint32\_t for inception on INCEPTION-EPOCH
-   [commit d3f6432](https://github.com/PowerDNS/pdns/commit/d3f6432): fix building on os x 10.9, thanks Martijn Bakker.
-   We now allow building against Lua 5.2 ([commit bef3000](https://github.com/PowerDNS/pdns/commit/bef3000), [commit 2bdd03b](https://github.com/PowerDNS/pdns/commit/2bdd03b), [commit 88d9e99](https://github.com/PowerDNS/pdns/commit/88d9e99)).
-   [commit fa1f845](https://github.com/PowerDNS/pdns/commit/fa1f845): autodetect MySQL 5.5+ connection charset
-   When misconfigured using 'right' timezones, a bug in (g)libc gmtime breaks our signatures. Fixed in [commit e4faf74](https://github.com/PowerDNS/pdns/commit/e4faf74) by Kees Monshouwer by implementing our own gmtime\_r.
-   When sending SERVFAIL due to a CNAME loop, don't uselessly include the CNAMEs ([commit dfd1b82](https://github.com/PowerDNS/pdns/commit/dfd1b82)).
-   Build fixes for platforms with 'weird' types (like s390/s390x): [commit c669f7c](https://github.com/PowerDNS/pdns/commit/c669f7c) ([details](http://blog.powerdns.com/2013/10/28/on-ragel-and-char-types/)), [commit 07b904e](https://github.com/PowerDNS/pdns/commit/07b904e) and [commit 2400764](https://github.com/PowerDNS/pdns/commit/2400764).
-   Support for += syntax for options, [commit 98dd325](https://github.com/PowerDNS/pdns/commit/98dd325) and others.
-   [commit f8f29f4](https://github.com/PowerDNS/pdns/commit/f8f29f4): nproxy: Add missing chdir("/") after chroot()
-   [commit 2e6e9ad](https://github.com/PowerDNS/pdns/commit/2e6e9ad): fix for "missing" libmysqlclient on RHEL/CentOS based systems
-   pdnssec check-zone improvements in [commit 5205892](https://github.com/PowerDNS/pdns/commit/5205892), [commit edb255f](https://github.com/PowerDNS/pdns/commit/edb255f), [commit 0dde9d0](https://github.com/PowerDNS/pdns/commit/0dde9d0), [commit 07ee700](https://github.com/PowerDNS/pdns/commit/07ee700), [commit 79a3091](https://github.com/PowerDNS/pdns/commit/79a3091), [commit 08f3452](https://github.com/PowerDNS/pdns/commit/08f3452), [commit bcf9daf](https://github.com/PowerDNS/pdns/commit/bcf9daf), [commit c9a3dd7](https://github.com/PowerDNS/pdns/commit/c9a3dd7), [commit 6ebfd08](https://github.com/PowerDNS/pdns/commit/6ebfd08), [commit fd53bd0](https://github.com/PowerDNS/pdns/commit/fd53bd0), [commit 7eaa83a](https://github.com/PowerDNS/pdns/commit/7eaa83a), [commit e319467](https://github.com/PowerDNS/pdns/commit/e319467), ,
-   NSEC/NSEC3 fixes in [commit 3191709](https://github.com/PowerDNS/pdns/commit/3191709), [commit f75293f](https://github.com/PowerDNS/pdns/commit/f75293f), [commit cd30e94](https://github.com/PowerDNS/pdns/commit/cd30e94), [commit 74baf86](https://github.com/PowerDNS/pdns/commit/74baf86), [commit 1fa8b2b](https://github.com/PowerDNS/pdns/commit/1fa8b2b)
-   The webserver could crash when the ring buffers were resized, fixed in [commit 3dfb45f](https://github.com/PowerDNS/pdns/commit/3dfb45f).
-   [commit 213ec4a](https://github.com/PowerDNS/pdns/commit/213ec4a): add constraints for name to pg schema
-   [commit f104427](https://github.com/PowerDNS/pdns/commit/f104427): make domainmetadata queries case insensitive
-   [commit 78fc378](https://github.com/PowerDNS/pdns/commit/78fc378): no label compression for name in TSIG records
-   [commit 15d6ffb](https://github.com/PowerDNS/pdns/commit/15d6ffb): pdnssec now outputs ZSK DNSKEY records if experimental-direct-dnskey support is enabled (renamed to direct-dnskey before release!)
-   [commit ad67d0e](https://github.com/PowerDNS/pdns/commit/ad67d0e): drop cryptopp from static build as libcryptopp.a is broken on Debian 7, which is what we build on
-   [commit 7632dd8](https://github.com/PowerDNS/pdns/commit/7632dd8): support polarssl 1.3 externally.
-   Remotebackend was fully updated in various commits.
-   [commit 82def39](https://github.com/PowerDNS/pdns/commit/82def39): SOA-EDIT: fix INCEPTION-INCREMENT handling
-   [commit a3a546c](https://github.com/PowerDNS/pdns/commit/a3a546c): add innodb-read-committed option to gmysql settings.
-   [commit 9c56e16](https://github.com/PowerDNS/pdns/commit/9c56e16): actually notice timeout during AXFR retrieve, thanks hkraal

# PowerDNS Recursor version 3.5.3
Released September 17th, 2013

This is a bugfix and performance update to 3.5.2. It brings serious performance improvements for dual stack users.

## Downloads
-   [Official download page](https://www.powerdns.com/downloads.html)
-   [native RHEL5/6 packages from Kees Monshouwer](http://www.monshouwer.eu/download/3rd_party/pdns-recursor/)

## Changes since 3.5.2
-   3.5 replaced our ANY query with A+AAAA for users with IPv6 enabled. Extensive measurements by Darren Gamble showed that this change had a non-trivial performance impact. We now do the ANY query like before, but fall back to the individual A+AAAA queries when necessary. Change in [commit 1147a8b](https://github.com/PowerDNS/pdns/commit/1147a8b).
-   The IPv6 address for d.root-servers.net was added in [commit 66cf384](https://github.com/PowerDNS/pdns/commit/66cf384), thanks Ralf van der Enden.
-   We now drop packets with a non-zero opcode (i.e. special packets like DNS UPDATE) earlier on. If the experimental pdns-distributes-queries flag is enabled, this fix avoids a crash. Normal setups were never susceptible to this crash. Code in [commit 35bc40d](https://github.com/PowerDNS/pdns/commit/35bc40d), closes [ticket 945](https://github.com/PowerDNS/pdns/issues/945).
-   TXT handling was somewhat improved in [commit 4b57460](https://github.com/PowerDNS/pdns/commit/4b57460), closing [ticket 795](https://github.com/PowerDNS/pdns/issues/795).

# PowerDNS Recursor version 3.5.2
Released June 7th, 2013

This is a stability and bugfix update to 3.5.1. It contains important fixes that improve operation for certain domains.

## Downloads
-   [Official download page](https://www.powerdns.com/downloads.html)
-   [native RHEL5/6 packages from Kees Monshouwer](http://www.monshouwer.eu/download/3rd_party/pdns-recursor/)

## Changes since 3.5.1
-   Responses without the QR bit set now get matched up to an outstanding query, so that resolution can be aborted early instead of waiting for a timeout. Code in [commit ee90f02](https://github.com/PowerDNS/pdns/commit/ee90f02).
-   The depth limiter changes in 3.5.1 broke some legal domains with lots of indirection. Improved in [commit d393c2d](https://github.com/PowerDNS/pdns/commit/d393c2d).
-   Slightly improved logging to aid debugging. Code in [commit 437824d](https://github.com/PowerDNS/pdns/commit/437824d) and [commit 182005e](https://github.com/PowerDNS/pdns/commit/182005e).

# PowerDNS Authoritative Server version 3.3
Released on July 5th 2013

This a stability, bugfix and conformity update to 3.2. It improves interoperability with various validators, either through bugfixes or by catering to their needs beyond the specifications.

**Warning**: Version 3.3 of the PowerDNS Authoritative Server is a major upgrade if you are coming from 2.9.x. There are also some important changes if you are coming from 3.0, 3.1 or 3.2. Please refer to the [Upgrade documentation](authoritative/upgrading.md) for important information on correct and stable operation, as well as notes on performance and memory use.

## Downloads
-   [Official download page](http://www.powerdns.com/content/downloads.html)
-   [native RHEL5/6 packages from Kees Monshouwer](http://www.monshouwer.eu/download/3rd_party/pdns-server/)

## Changes between RC2 and final
-   pdnssec rectify-zone now refuses to operate on presigned zones, as rectification already happens during incoming transfer. Patch by Kees Monshouwer in [commit 9bd211e](https://github.com/PowerDNS/pdns/commit/9bd211e).
-   We now handle zones with a mix of NSEC3 opt-out and non-opt-out ranges correctly during inbound and outbound AXFR. Many thanks to Kees Monshouwer. Code in [commit 5aa7003](https://github.com/PowerDNS/pdns/commit/5aa7003) and [commit d3e7b17](https://github.com/PowerDNS/pdns/commit/d3e7b17).
-   More remotebackend fixes ([commit 32d4f44](https://github.com/PowerDNS/pdns/commit/32d4f44), [commit 44c2ee8](https://github.com/PowerDNS/pdns/commit/44c2ee8), [commit 1fcc7b7](https://github.com/PowerDNS/pdns/commit/1fcc7b7), [commit 0b1a3b2](https://github.com/PowerDNS/pdns/commit/0b1a3b2), [commit 9a319b1](https://github.com/PowerDNS/pdns/commit/9a319b1)), thanks Aki Tuomi.
-   Some compiler warnings were squashed ([commit ed554db](https://github.com/PowerDNS/pdns/commit/ed554db)), thanks Morten Stevens.
-   Fix broken memory access in LOC parser ([commit 4eec51b](https://github.com/PowerDNS/pdns/commit/4eec51b), [commit bea513c](https://github.com/PowerDNS/pdns/commit/bea513c)), thanks Aki Tuomi.
-   DNSSEC: DS queries at the apex of a zone for which we are not hosting the parent, would wrongly get an 'unauth NOERROR'. Fixed by Kees Monshouwer in [commit 34479a6](https://github.com/PowerDNS/pdns/commit/34479a6).

## Changes between RC1 and RC2
-   Added dnstcpbench tool, by popular demand.
-   We always shipped a static tools RPM; we now have a similar Debian package. All packages have been cleaned up a bit, and the binary collections are now consistent between RPM and Deb. New: pass --enable-tools to configure to have the tools included in 'make all' and 'make install'.
-   [commit 4d2e3f5](https://github.com/PowerDNS/pdns/commit/4d2e3f5): add selinux policy files
-   We would sometimes send a single NULL byte, or nothing at all, instead of an OPT record. Fixed in [commit bf7f822](https://github.com/PowerDNS/pdns/commit/bf7f822), [commit 063076b](https://github.com/PowerDNS/pdns/commit/063076b), [commit 90d361d](https://github.com/PowerDNS/pdns/commit/90d361d).
-   [commit 2ee9ba2](https://github.com/PowerDNS/pdns/commit/2ee9ba2): expand any-to-tcp to direct RRSIG queries
-   [commit 5fff084](https://github.com/PowerDNS/pdns/commit/5fff084), [commit e38ef51](https://github.com/PowerDNS/pdns/commit/e38ef51): drop no-op flag strict-rfc-axfrs, thanks Jelte Jansen.
-   [commit f3d8902](https://github.com/PowerDNS/pdns/commit/f3d8902), [commit 7c0b859](https://github.com/PowerDNS/pdns/commit/7c0b859), [commit 5eea730](https://github.com/PowerDNS/pdns/commit/5eea730): Implement MINFO qtype for better interaction when slaving zones from NSD (that contain MINFO). Thanks to Jelte Jansen.
-   [commit 8655a42](https://github.com/PowerDNS/pdns/commit/8655a42), [commit bf79c6a](https://github.com/PowerDNS/pdns/commit/bf79c6a), [commit 38c941b](https://github.com/PowerDNS/pdns/commit/38c941b): SRV record can have a '.' as final field, from which we would dutifully strip the trailing ., leaving void, confusing everything. We now remove the trailing . in the right place, and not if we are trying to server '.'. Again thanks to Jelte & SIDN for catching this.
-   [commit 70d5a66](https://github.com/PowerDNS/pdns/commit/70d5a66): improve error message in ill formed unknown record type, thanks Jelte Jansen for reporting.
-   [commit 3640473](https://github.com/PowerDNS/pdns/commit/3640473): Built in webserver can now listen on IPv6, fixes [ticket 843](https://github.com/PowerDNS/pdns/issues/843). Also silences some useless messages about timeouts.
-   [commit 7db735c](https://github.com/PowerDNS/pdns/commit/7db735c), [commit d72166c](https://github.com/PowerDNS/pdns/commit/d72166c): CHANGES BEHAVIOUR: before we launch, check if we can connect to the controlsocket we are about to obliterate. If it works, abort. Fixes [ticket 841](https://github.com/PowerDNS/pdns/issues/841) and changes standing behaviour. There might be circumstances where PowerDNS now refuses to start, where it previously would. However, starting and making our previous instance mute wasn't good.
-   [commit 9130f9e](https://github.com/PowerDNS/pdns/commit/9130f9e): correctly refuse out-of-zone data in bindbackend, closes [ticket 845](https://github.com/PowerDNS/pdns/issues/845)
-   [commit 3363ef7](https://github.com/PowerDNS/pdns/commit/3363ef7): initialise server-id after all parsing is done, instead of half way through. Fixes situations where server-id was emptied explicitly. Reported by Wouter de Jong
-   [commit cd4f253](https://github.com/PowerDNS/pdns/commit/cd4f253): bump boost requirement, thanks Wouter de Jong
-   [commit 58cad74](https://github.com/PowerDNS/pdns/commit/58cad74): Update pdns auth init script so it works on wheezy
-   [commit 8714c9c](https://github.com/PowerDNS/pdns/commit/8714c9c): clang fixes by Aki Tuomi, thanks!
-   [commit 146601d](https://github.com/PowerDNS/pdns/commit/146601d): stretch supermasters.ip for IPv6, thanks Dennis Krul
-   [commit 1a5c5f9](https://github.com/PowerDNS/pdns/commit/1a5c5f9): various remotebackend improvements by Aki Tuomi
-   [commit 6ab1a11](https://github.com/PowerDNS/pdns/commit/6ab1a11): make sure systemd starts PowerDNS after relevant databases have been started, thanks Morten Stevens.
-   [commit 606018f](https://github.com/PowerDNS/pdns/commit/606018f), [commit ee5e175](https://github.com/PowerDNS/pdns/commit/ee5e175), [commit c76f6f4](https://github.com/PowerDNS/pdns/commit/c76f6f4): check scopeMask of answer packet, not of query packet!
-   [commit 2b18bcf](https://github.com/PowerDNS/pdns/commit/2b18bcf): Added warning if trailing dot is used, thanks Aki Tuomi.
-   [commit 16cf913](https://github.com/PowerDNS/pdns/commit/16cf913): make superfluous 'bind' NSEC3 record optional

## New features and important changes since 3.2 (these changes are in RC1 and up)
-   [commit 04576ee](https://github.com/PowerDNS/pdns/commit/04576ee), [commit b0e15c8](https://github.com/PowerDNS/pdns/commit/b0e15c8): Implement pdnssec increase-serial, thanks Ruben d'Arco.
-   [commit cee857b](https://github.com/PowerDNS/pdns/commit/cee857b): PowerDNS now sets additional groups while dropping privileges.
-   [commit 7796a3b](https://github.com/PowerDNS/pdns/commit/7796a3b): Merge support for include-dir directive, thanks Aki Tuomi!
-   [commit d725755](https://github.com/PowerDNS/pdns/commit/d725755): make pdns-static Conflict with pdns-server, closes [ticket 640](https://github.com/PowerDNS/pdns/issues/640)
-   [commit c0d5504](https://github.com/PowerDNS/pdns/commit/c0d5504): pdnssec now emits 'INSERT INTO domain ..' queries when running without named.conf, thanks Ruben d'Arco.
-   [commit a1d6b0c](https://github.com/PowerDNS/pdns/commit/a1d6b0c): Older versions of the BIND 9 validating recursor need a superfluous NSEC3 record on positive wildcard responses. We now send this extra NSEC3. Closes [ticket 814](https://github.com/PowerDNS/pdns/issues/814).
-   [commit 07bf35d](https://github.com/PowerDNS/pdns/commit/07bf35d): catch a lot more errors in pdnssec and report them. Fixes [ticket 588](https://github.com/PowerDNS/pdns/issues/588).
-   [commit 032e390](https://github.com/PowerDNS/pdns/commit/032e390): make pdnssec exit with 1 on some error conditions, closes [ticket 677](https://github.com/PowerDNS/pdns/issues/677)
-   [commit 4af49b8](https://github.com/PowerDNS/pdns/commit/4af49b8), [commit 4cec6ac](https://github.com/PowerDNS/pdns/commit/4cec6ac): add ability to create an 'active' or inactive key using add-zone-key and import-zone-key, plus silenced some debugging. Fixes [ticket 707](https://github.com/PowerDNS/pdns/issues/707).
-   [commit fae4167](https://github.com/PowerDNS/pdns/commit/fae4167): Compiling against Lua 5.2 (--with-lua=lua5.2) now disables some code used for regression testing, instead of breaking during compile. This means that Lua 5.2 can be used in production.
-   [commit abc8f3f](https://github.com/PowerDNS/pdns/commit/abc8f3f), [357f6a7](https://github.com/PowerDNS/pdns/commit/357f6a7): Implement the new any-to-tcp option that, when set, always replies with a truncated response (TC=1) to ANY queries, forcing them to use TCP.
-   [commit 496073b](https://github.com/PowerDNS/pdns/commit/496073b): Since 3.0, pdnssec secure-zone has always generated 3 keys: one KSK and two ZSK, with one ZSK active. For most, if not almost all, users, this inactive ZSK is never used. We now no longer generate this useless ZSK. The resulting smaller DNSKEY RRset improves interoperability with certain validators. Closes [ticket 824](https://github.com/PowerDNS/pdns/issues/824).
-   [commit df55450](https://github.com/PowerDNS/pdns/commit/df55450): Non-DNSSEC ANY queries no longer get sent DNSSEC records. This improves interoperability with some old resolvers. Patch by Kees Monshouwer.
-   [commit 04b4bf6](https://github.com/PowerDNS/pdns/commit/04b4bf6): Merge support for not using opt-out with NSEC3. Many thanks to Kees Monshouwer.
-   [commit 8db49a6](https://github.com/PowerDNS/pdns/commit/8db49a6): We now try not to NOTIFY ourselves. In convoluted cases involving REUSE\_PORT and binding to 0.0.0.0 and ::, it might be possible that we guess wrong, in which case you can set prevent-self-notification to off.

## Important bug fixes
-   [commit 63e365d](https://github.com/PowerDNS/pdns/commit/63e365d): don't mess up encoding when copying qname from question to answer in packetcache. Based on reports&debugging by Jimmy Bergman (sigint), Daniel Norman (Loopia) and the fine people at ISC. This avoids most issues related to BIND 9 erroneously blacklisting PowerDNS for lack of EDNS support.
-   [commit 3526186](https://github.com/PowerDNS/pdns/commit/3526186): fix backslash handling in TXT parser, includes test. Thanks Jan-Piet Mens.
-   [commit 830281f](https://github.com/PowerDNS/pdns/commit/830281f), [aef7330](https://github.com/PowerDNS/pdns/commit/aef7330): Accept chars \>127 ('high ASCII') in TXT records, closing [ticket 541](https://github.com/PowerDNS/pdns/issues/541) and [723](https://github.com/PowerDNS/pdns/issues/723).
-   [commit feef1ec](https://github.com/PowerDNS/pdns/commit/feef1ec): fix missing NSEC3 for secure delegation, thanks Kees Monshouwer, closes [ticket 682](https://github.com/PowerDNS/pdns/issues/682)
-   [commit b61e407](https://github.com/PowerDNS/pdns/commit/b61e407): around Thursday midnight, during signature rollovers, we would update the SOA serial too early. Fixed by reverting [commit d90efbf](https://github.com/PowerDNS/pdns/commit/d90efbf), adding 7 days margin to inception. Fix by Kees Monshouwer.
-   [commit ff64750](https://github.com/PowerDNS/pdns/commit/ff64750): make sure mixed-case queries get a correct apex NSEC3 type bitmap
-   [commit 4b153d8](https://github.com/PowerDNS/pdns/commit/4b153d8): always lowercase next name in NSEC to avoid interop troubles with validators, thanks Marco Davids&Matthijs Mekking.

## Other changes
-   [commit 49977c6](https://github.com/PowerDNS/pdns/commit/49977c6): fix bug in boost.m4 where it insists on setting -L, causing useless RPATH in our binaries. Closes [ticket 728](https://github.com/PowerDNS/pdns/issues/728)
-   [commit 62ac758](https://github.com/PowerDNS/pdns/commit/62ac758): use PolarSSL for MD5 hashing instead of shipping our own copy of md5 hashing code, thanks Aki Tuomi.
-   [commit 775acd9](https://github.com/PowerDNS/pdns/commit/775acd9): give a better error on trying to add nsec3 parameters to a weird zone like "1 0 1 ab" (which indicates that you forgot to specify a zone name on the command line). Fixes [ticket 800](https://github.com/PowerDNS/pdns/issues/800).
-   [commit 315dd2e](https://github.com/PowerDNS/pdns/commit/315dd2e): Simplify socket listening code, and make sure we always set the nonblocking flag correctly. Patch by Mark Zealey, closes [ticket 664](https://github.com/PowerDNS/pdns/issues/664).
-   [commit b35da1b](https://github.com/PowerDNS/pdns/commit/b35da1b): if\_ether.h is in netinet/ not net/ on OpenBSD, thanks Florian Obser.
-   [commit 71301b6](https://github.com/PowerDNS/pdns/commit/71301b6): Replicate gsql backend feature of having separate -auth queries for DNSSEC into oraclebackend. Also lets you disable dnssec if you are not ready for it. Closes [ticket 527](https://github.com/PowerDNS/pdns/issues/527), patch by Aki Tuomi.
-   [commit 2125dac](https://github.com/PowerDNS/pdns/commit/2125dac): drop unused ignore-rd-bit flag
-   [commit 8c1a6d6](https://github.com/PowerDNS/pdns/commit/8c1a6d6): NSECx optimizations, thanks Kees Monshouwer.
-   [commit 664716a](https://github.com/PowerDNS/pdns/commit/664716a): drop unused variables in lua backend ( [ticket 653](https://github.com/PowerDNS/pdns/issues/653))
-   [commit d8ec70f](https://github.com/PowerDNS/pdns/commit/d8ec70f): fix db2 backend includes ( [ticket 653](https://github.com/PowerDNS/pdns/issues/653))
-   [commit 6477102](https://github.com/PowerDNS/pdns/commit/6477102): add goracle schema, thanks Aki Tuomi.
-   [commit 9118638](https://github.com/PowerDNS/pdns/commit/9118638): make goraclebackend "at least work", closes [ticket 729](https://github.com/PowerDNS/pdns/issues/729), thanks Aki Tuomi.
-   [commit e0ad7bb](https://github.com/PowerDNS/pdns/commit/e0ad7bb): add DS digest type 4 to show-zone output; add algorithm names. Based on a patch by Aki Tuomi, closes [ticket 744](https://github.com/PowerDNS/pdns/issues/744)
-   [commit 61a7fac](https://github.com/PowerDNS/pdns/commit/61a7fac): enable AM\_SILENT\_RULES, closing [ticket 647](https://github.com/PowerDNS/pdns/issues/647)
-   [commit 837f4b4](https://github.com/PowerDNS/pdns/commit/837f4b4): do a better job at escaping TXT, fixes [ticket 795](https://github.com/PowerDNS/pdns/issues/795)
-   [commit 6ca3fa7](https://github.com/PowerDNS/pdns/commit/6ca3fa7): add SOA-EDIT INCEPTION-INCREMENT mode, thanks stbuehler
-   [commit 6159c49](https://github.com/PowerDNS/pdns/commit/6159c49): Add connection info to sql-connect message
-   [commit 9f62e34](https://github.com/PowerDNS/pdns/commit/9f62e34), [commit 0fc965f](https://github.com/PowerDNS/pdns/commit/0fc965f), [commit 2035112](https://github.com/PowerDNS/pdns/commit/2035112): Added EUI48 and EUI64 record types
-   [commit f9cf6d9](https://github.com/PowerDNS/pdns/commit/f9cf6d9): cut the number of database queries in half for AXFR-in, thanks Kees Monshouwer.
-   [commit c87f987](https://github.com/PowerDNS/pdns/commit/c87f987): add default for SOA contact e-mail
-   [commit bb4a573](https://github.com/PowerDNS/pdns/commit/bb4a573): move random backend to modules, thanks Kees Monshouwer.
-   [commit 1071abd](https://github.com/PowerDNS/pdns/commit/1071abd): restyle builtin webserver page, thanks Christian Hofstaedtler.
-   [commit cd5e158](https://github.com/PowerDNS/pdns/commit/cd5e158): correct bogus use of poll(2) related constants, improving non-Linux portability. Thanks Wouter de Jong.
-   [commit 27ff60a](https://github.com/PowerDNS/pdns/commit/27ff60a): make sure our NSEC(3)s for names with spaces in them are correct. Reported by Jimmy Bergman. Includes test.
-   [commit 116e28a](https://github.com/PowerDNS/pdns/commit/116e28a): reduce log level of successful gpgsql/gsqlite3 connection to Info
-   [commit b23b90a](https://github.com/PowerDNS/pdns/commit/b23b90a): Metadata update is now in the same transaction as the AXFR. This improves slaving speed tremendously, especially for SQLite users. Patch by Kees Monshouwer.
-   [commit 4620e8a](https://github.com/PowerDNS/pdns/commit/4620e8a): Added zone2json, thanks Aki Tuomi.
-   [commit f0fa8b6](https://github.com/PowerDNS/pdns/commit/f0fa8b6): Fix remotebackend setdomainmetadata return value handling. Fix by Aki Tuomi, closes [ticket 740](https://github.com/PowerDNS/pdns/issues/740).
-   [commit 80e82d6](https://github.com/PowerDNS/pdns/commit/80e82d6): log control listener abort even more explicitly.
-   [commit 7c0cb15](https://github.com/PowerDNS/pdns/commit/7c0cb15), [a718d74](https://github.com/PowerDNS/pdns/commit/a718d74): support automake 1.12
-   [commit 3fe22eb](https://github.com/PowerDNS/pdns/commit/3fe22eb), [6707cb1](https://github.com/PowerDNS/pdns/commit/6707cb1): update autoconf/automake preamble to non-deprecated variant, thanks Morten Stevens
-   [commit 6c4e531](https://github.com/PowerDNS/pdns/commit/6c4e531): disarm dead code that causes gcc crashes on ARM, thanks Morten Stevens.
-   [commit 36855b5](https://github.com/PowerDNS/pdns/commit/36855b5): if we failed to make a new UDP socket, we'd report a confusing error about it.
-   [commit 1b8e5e6](https://github.com/PowerDNS/pdns/commit/1b8e5e6): autoconf support for oracle, thanks Aki Tuomi. Closes [ticket 726](https://github.com/PowerDNS/pdns/issues/726).
-   [commit 8ac0c06](https://github.com/PowerDNS/pdns/commit/8ac0c06): allow setting of some oracle env vars. Patch by Aki Tuomi, closes [ticket 725](https://github.com/PowerDNS/pdns/issues/725).
-   [commit 45e845b](https://github.com/PowerDNS/pdns/commit/45e845b): add example.rb sample script for remotebackend, thanks Aki Tuomi.
-   [commit 950bddd](https://github.com/PowerDNS/pdns/commit/950bddd): add pdnssec generate-zone-key command, thanks Aki. Closes [ticket 711](https://github.com/PowerDNS/pdns/issues/711).
-   [commit 2c03cde](https://github.com/PowerDNS/pdns/commit/2c03cde): Replace select with waitForData in remotebackend. Patch by Aki Tuomi, closes [ticket 715](https://github.com/PowerDNS/pdns/issues/715).
-   [commit 450292c](https://github.com/PowerDNS/pdns/commit/450292c): accept ANY responses during recursive forwarding, thanks Jan-Piet Mens.
-   [commit d9dd76b](https://github.com/PowerDNS/pdns/commit/d9dd76b): actually clean up unix domain sockets too after use.
-   [commit 36758d2](https://github.com/PowerDNS/pdns/commit/36758d2): merge [ticket 476](https://github.com/PowerDNS/pdns/issues/476) by Aki Tuomi, providing default-ksk/zsk-algorithms/size configuration parameters for pdnssec.
-   [commit 2f2b014](https://github.com/PowerDNS/pdns/commit/2f2b014): apply variant of code in [ticket 714](https://github.com/PowerDNS/pdns/issues/714) so we can lauch pipe backend scripts with parameters, plus add experimental code that if pipe-command is a unix domain socket, we use that.
-   [commit 9566683](https://github.com/PowerDNS/pdns/commit/9566683): merge patch from ticket 712 addressing memory leak in remotebackend, thanks Aki.
-   [commit fb6ed6f](https://github.com/PowerDNS/pdns/commit/fb6ed6f): explicitly set domain id during bindbackend superslave domain create, thanks Kees Monshouwer&Aki Tuomi.
-   [commit 69bae20](https://github.com/PowerDNS/pdns/commit/69bae20): use private temp dir when running under systemd, thanks Morten Stevens&Ruben Kerkhof.
-   [commit b26a48a](https://github.com/PowerDNS/pdns/commit/b26a48a): fix rapidjson usage in remotebackend, patch by Aki Tuomi. Closes [ticket 697](https://github.com/PowerDNS/pdns/issues/697).
-   [commit da8e6ae](https://github.com/PowerDNS/pdns/commit/da8e6ae): also answer questions with : in them.
-   [commit ef1c4bf](https://github.com/PowerDNS/pdns/commit/ef1c4bf): also spot trailing dots on CNAME content, thanks Jan-Piet Mens and Ruben d'Arco.
-   [commit fb31631](https://github.com/PowerDNS/pdns/commit/fb31631): only setCloseOnExec on valid sockets

# PowerDNS Recursor version 3.5.1
Released May 3rd, 2013

This is a stability and bugfix update to 3.5. It contains important fixes that improve operation for certain domains.

## Downloads
-   [Official download page](https://www.powerdns.com/downloads.html)
-   [native RHEL5/6 packages from Kees Monshouwer](http://www.monshouwer.eu/download/3rd_party/pdns-recursor/)

## Changes since 3.5

-   We now abort earlier while following endless glue or CNAME chains. Fix in [commit 02d1742](https://github.com/PowerDNS/pdns/commit/02d1742).
-   Some unused code would crash certain gcc versions on ARM. Reported by Morten Stevens, fixed in [commit 5b188e8](https://github.com/PowerDNS/pdns/commit/5b188e8).
-   The 3.5 fix for [ticket 731](https://github.com/PowerDNS/pdns/issues/731) was too strict, causing trouble with at least one domain. Reported by Aki Tuomi, check slightly relaxed in [commit 4134690](https://github.com/PowerDNS/pdns/commit/4134690).
-   Automake/autoconf now use non-deprecated syntax. Reported by Morten Stevens, change in [commit ca17ef2](https://github.com/PowerDNS/pdns/commit/ca17ef2).

# PowerDNS Recursor version 3.5
Released April 15th, 2013

This is a stability, security and bugfix update to 3.3/3.3.1. It contains important fixes for slightly broken domain names, which your users expect to work anyhow.
**Note**: Because a semi-sanctioned 3.4-pre was distributed for a long time, and people have come to call that 3.4, we are skipping an actual 3.4 release to avoid confusion.

## Downloads
-   [Official download page](https://www.powerdns.com/downloads.html)
-   [native RHEL5/6 packages from Kees Monshouwer](http://www.monshouwer.eu/download/3rd_party/pdns-recursor/)

## Changes between RC5 and the final 3.5 release
-   Winfried Angele reported that restarting a very busy recursor could lead to crashes. Fixed in r3153, closing [ticket 735](https://github.com/PowerDNS/pdns/issues/735).

## Changes between RC4 and RC5
-   Bernd-René Predota of Liberty Global reported that Recursor 3.3 would treat empty non-AA NOERROR responses as authoritative NXDATA responses. This bug turned out to be in 3.5-RC4 too. Fixed in [commit 3146](http://wiki.powerdns.com/projects/trac/changeset/3146), related to [ticket 731](https://github.com/PowerDNS/pdns/issues/731).

## Changes between RC3 (unreleased) and RC4
-   Winfried Angele spotted, even before release, that [commit 3132](http://wiki.powerdns.com/projects/trac/changeset/3132) in RC3 broke outgoing IPv6 queries. We are grateful for his attention to detail! Fixed in [commit 3141](http://wiki.powerdns.com/projects/trac/changeset/3141).
Changes between RC2 and RC3 (unreleased)
-   Use private temp dir when running under systemd, thanks Morten Stevens and Ruben Kerkhof. Change in [commit 3105](http://wiki.powerdns.com/projects/trac/changeset/3105).
-   NSD mistakenly compresses labels for RP and other types, violating a MUST in RFC 3597. Recursor does not decompress these labels, violating a SHOULD in RFC3597. We now decompress these labels, and reportedly NSD will stop compressing them. Reported by Jan-Piet Mens, fixed in [commit 3109](http://wiki.powerdns.com/projects/trac/changeset/3109).
-   When forwarding to another recursor, we would handle responses to ANY queries incorrectly. Spotted by Jan-Piet Mens, fixed in [commit 3116](http://wiki.powerdns.com/projects/trac/changeset/3116), closes [ticket 704](https://github.com/PowerDNS/pdns/issues/704).
-   Our local-nets definition (used as a default for some settings) now includes the networks from RFC 3927 and RFC 6598. Reported by Maik Zumstrull, fixed in [commit 3122](http://wiki.powerdns.com/projects/trac/changeset/3122).
-   The RC1 change to stop using ANY queries to get A+AAAA for name servers in one go had a 5% performance impact. This impact is corrected in [commit 3132](http://wiki.powerdns.com/projects/trac/changeset/3132). Thanks to Winfried Angele for measuring and reporting this. Closes [ticket 710](https://github.com/PowerDNS/pdns/issues/710).
-   New command 'rec\_control dump-nsspeeds' will dump our NS speeds (latency) cache. Code in [commit 3131](http://wiki.powerdns.com/projects/trac/changeset/3131).

## Changes between RC1 and RC2
-   While Recursor 3.3 was not vulnerable to the specific attack noted in 'Ghost Domain Names: Revoked Yet Still Resolvable' (more information at [A New DNS Exploitation Technique: Ghost Domain Names](http://resources.infosecinstitute.com/ghost-domain-names/)), further investigation showed that a variant of the attack could work. This was fixed in [commit 3085](http://wiki.powerdns.com/projects/trac/changeset/3085). This should also close the slightly bogus [CVE-2012-1193](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2012-1193). Closes [ticket 668](https://github.com/PowerDNS/pdns/issues/668).
-   The auth-can-lower-ttl flag was removed, as it did not have any effect in most situations, and thus did not operate as advertised. We now always comply with the related parts of RFC 2181. Change in [commit 3092](http://wiki.powerdns.com/projects/trac/changeset/3092), closing [ticket 88](https://github.com/PowerDNS/pdns/issues/88).

## New features
-   The local zone server now understands wildcards, code in [commit 2062](http://wiki.powerdns.com/projects/trac/changeset/2062).
-   The Lua postresolve and nodata hooks, that had been distributed as a '3.3-hooks' snapshot earlier, have been merged. Code in [commit 2309](http://wiki.powerdns.com/projects/trac/changeset/2309).
-   A new feature, rec\_control trace-regex allows the tracing of lookups for specific names. Code in [commit 3044](http://wiki.powerdns.com/projects/trac/changeset/3044), [commit 3073](http://wiki.powerdns.com/projects/trac/changeset/3073).
-   A new setting, export-etc-hosts-search-suffix, adds a configurable suffix to names imported from /etc/hosts. Code in [commit 2544](http://wiki.powerdns.com/projects/trac/changeset/2544), [commit 2545](http://wiki.powerdns.com/projects/trac/changeset/2545).

## Improvements
-   We now throttle queries that don't work less aggressively, code in [commit 1766](http://wiki.powerdns.com/projects/trac/changeset/1766).
-   Various improvements in tolerance against broken auths, code in [commit 1996](http://wiki.powerdns.com/projects/trac/changeset/1996), [commit 2188](http://wiki.powerdns.com/projects/trac/changeset/2188), [commit 3074](http://wiki.powerdns.com/projects/trac/changeset/3074) (thanks Winfried).
-   Additional processing is now optional, and disabled by default. Presumably this yields a performance improvement. Change in [commit 2542](http://wiki.powerdns.com/projects/trac/changeset/2542).
-   rec\_control reload-lua-script now reports errors. Code in [commit 2627](http://wiki.powerdns.com/projects/trac/changeset/2627), closing [ticket 278](https://github.com/PowerDNS/pdns/issues/278).
-   rec\_control help now lists commands. Code in [commit 2628](http://wiki.powerdns.com/projects/trac/changeset/2628).
-   rec\_control wipe-cache now also wipes the recursor's packet cache. Code in [commit 2880](http://wiki.powerdns.com/projects/trac/changeset/2880) from [ticket 333](https://github.com/PowerDNS/pdns/issues/333).
-   Morten Stevens contributed a systemd file. Import in [commit 2966](http://wiki.powerdns.com/projects/trac/changeset/2966), now part of the recursor tarball.
-   [commit 2990](http://wiki.powerdns.com/projects/trac/changeset/2990) updates the address of D.root-servers.net.
-   Winfried Angele implemented and documented the ipv6-questions metric. Merge in [commit 3034](http://wiki.powerdns.com/projects/trac/changeset/3034), closing [ticket 619](https://github.com/PowerDNS/pdns/issues/619).
-   We no longer use ANY to get A+AAAA for nameservers, because some auth operators have decided to break ANY lookups. As a bonus, we now track v4 and v6 latency separately. Change in [commit 3064](http://wiki.powerdns.com/projects/trac/changeset/3064).

## Bugs fixed
-   Some unaligned memory access was corrected, code in [commit 2060](http://wiki.powerdns.com/projects/trac/changeset/2060), [commit 2122](http://wiki.powerdns.com/projects/trac/changeset/2122), [commit 2123](http://wiki.powerdns.com/projects/trac/changeset/2123), which would cause problems on UltraSPARC.
-   Garbage encountered during reload-acls could cause crashes. Fixed in [commit 2323](http://wiki.powerdns.com/projects/trac/changeset/2323), closing [ticket 330](https://github.com/PowerDNS/pdns/issues/330).
-   The recursor would lose its root hints in a very rare situation. Corrected in [commit 2380](http://wiki.powerdns.com/projects/trac/changeset/2380).
-   We did not always drop supplemental groups while dropping privileges. Reported by David Black of Atlassian, fixed in [commit 2524](http://wiki.powerdns.com/projects/trac/changeset/2524).
-   Cache aging would sometimes get confused when we had a mix of expired and non-expired records in cache. Spotted and fixed by Winfried Angele in [commit 3068](http://wiki.powerdns.com/projects/trac/changeset/3068), closing [ticket 438](https://github.com/PowerDNS/pdns/issues/438).
-   rec\_control reload-acl no longer ignores arguments. Fix in [commit 3037](http://wiki.powerdns.com/projects/trac/changeset/3037), closing [ticket 490](https://github.com/PowerDNS/pdns/issues/490).
-   Since we re-parse our commandline in rec\_control we've been doubling the commands on the commandline, causing weird output. Reported by Winfried Angele. Fixed in [commit 2992](http://wiki.powerdns.com/projects/trac/changeset/2992), closing [ticket 618](https://github.com/PowerDNS/pdns/issues/618). This issue was not present in any officially released versions.
-   [commit 2879](http://wiki.powerdns.com/projects/trac/changeset/2879) drops some spurious stderr logging from Lua scripts, and makes sure 'place' is always valid.
-   We would sometimes refuse to resolve domains with just one nameserver living at the apex. Fixed in [commit 2817](http://wiki.powerdns.com/projects/trac/changeset/2817).
-   We would sometimes stick RRs in the wrong parts of response packets. Fixed in [commit 2625](http://wiki.powerdns.com/projects/trac/changeset/2625).
-   The ACL parser was too liberal, sometimes causing recursors to be very open. Fixed in [commit 2629](http://wiki.powerdns.com/projects/trac/changeset/2629), closing [ticket 331](https://github.com/PowerDNS/pdns/issues/331).
-   rec\_control now honours socket-dir from recursor.conf. Fixed in [commit 2630](http://wiki.powerdns.com/projects/trac/changeset/2630).
-   When traversing CNAME chains, sometimes we would end up with multiple SOAs in the result. Fixed in [commit 2633](http://wiki.powerdns.com/projects/trac/changeset/2633).

# PowerDNS Authoritative Server 3.2
Released January 17th, 2013

This is a stability and conformity update to 3.1. It mostly makes our DNSSEC implementation more robust, and improves interoperability with various validators. 3.2 has received very extensive testing on a lot of edge cases, verifying output both against common validators and compared against other authoritative servers.

**Warning**: Version 3.2 of the PowerDNS Authoritative Server is a major upgrade if you are coming from 2.9.x. There are also some important changes if you are coming from 3.0 or 3.1. Please refer to the [Upgrade documentation](authoritative/upgrading.md) for important information on correct and stable operation, as well as notes on performance and memory use.

## Downloads
-   [Official download page](http://www.powerdns.com/content/downloads.html)
-   [native RHEL5/6 packages from Kees Monshouwer](http://www.monshouwer.eu/download/3rd_party/pdns-server/)
-   [additional third-party builds](http://wiki.powerdns.com/trac#GettingPowerDNSpackages)

In addition to all the changes below, we now auto-build semi-static packages. Relevant changes to make that possible are in [commit 2849](http://wiki.powerdns.com/projects/trac/changeset/2849), [commit 2853](http://wiki.powerdns.com/projects/trac/changeset/2853), 2858, [commit 2859](http://wiki.powerdns.com/projects/trac/changeset/2859), [commit 2860](http://wiki.powerdns.com/projects/trac/changeset/2860).

## Changes between 3.2-RC4 and the final 3.2 release
-   Aki Tuomi contributed a bunch of fixes to our crypto drivers. Code in [commit 3036](http://wiki.powerdns.com/projects/trac/changeset/3036) and [commit 3055](http://wiki.powerdns.com/projects/trac/changeset/3055)/[commit 3057](http://wiki.powerdns.com/projects/trac/changeset/3057).
-   The ksk|zsk argument for pdnssec import-zone-key was required while it should be optional. Fixed in [commit 3051](http://wiki.powerdns.com/projects/trac/changeset/3051).

## Changes between 3.2-RC3 and 3.2-RC4
-   The experimental undocumented bindbackend superslave mode would break the first added domain until a restart. Fixed by Kees Monshouwer in [commit 3013](http://wiki.powerdns.com/projects/trac/changeset/3013).
-   Sander Hoentjen reported an issue with our choice of ports for outgoing TCP connections. Investigating it turned up that we were randomizing TCP connections on purpose while leaving UDP port choice to the kernel, which should be the other way around. Fixed in [commit 3014](http://wiki.powerdns.com/projects/trac/changeset/3014), closing [ticket 643](https://github.com/PowerDNS/pdns/issues/643) and [ticket 644](https://github.com/PowerDNS/pdns/issues/644).
-   Aki Tuomi contributed some autoconf code to use mysql\_config if it is available. Code in [commit 3015](http://wiki.powerdns.com/projects/trac/changeset/3015) and [commit 3019](http://wiki.powerdns.com/projects/trac/changeset/3019), closing [ticket 458](https://github.com/PowerDNS/pdns/issues/458).
-   The MongoDB backend was removed at the author's request, as it does not work with any current libmongo versions. Change in [commit 3017](http://wiki.powerdns.com/projects/trac/changeset/3017).
-   Mark Zealey discovered we were retrieving the ascii powerdns version string for each packet, not just for version string queries. Fixed in [commit 3018](http://wiki.powerdns.com/projects/trac/changeset/3018), closing [ticket 651](https://github.com/PowerDNS/pdns/issues/651).
-   Our new json code would not compile on solaris 9 and 10 due to lack of strcasestr. Juraj Lutter contributed a portable version in [commit 3020](http://wiki.powerdns.com/projects/trac/changeset/3020).
-   Mark Zealey noted that RRs with low TTLs could lower our query-cache-ttl persistently. Fixed in [commit 3023](http://wiki.powerdns.com/projects/trac/changeset/3023), closing [ticket 662](https://github.com/PowerDNS/pdns/issues/662).
-   pdnssec now honours module-dir, patch by Fredrik Danerklint in [commit 3026](http://wiki.powerdns.com/projects/trac/changeset/3026).

## Changes between 3.2-RC2 and 3.2-RC3
-   Michael Scheffler noticed that the lazy-recursion setting had no effect at all. Setting removed in [commit 3003](http://wiki.powerdns.com/projects/trac/changeset/3003).
-   Mark Zealey found that an earlier performance improvement could cause crashes under high load, with lots of IPs configured in local-address and receiver-threads higher than 1. Fixed in [commit 3005](http://wiki.powerdns.com/projects/trac/changeset/3005).

## Changes between 3.2-RC1 and 3.2-RC2
-   The udp-queries metric would only count on the first thread launched, instead of on all threads. Additionally, it was initialised at MAXINT at startup, instead of at 0. Both issues fixed by Kees Monshouwer in [commit 2999](http://wiki.powerdns.com/projects/trac/changeset/2999), closing [ticket 491](https://github.com/PowerDNS/pdns/issues/491) and [ticket 582](https://github.com/PowerDNS/pdns/issues/582).
-   Aki Tuomi contributed zone2json, a great way for programmers to benefit from our zone file parser. Code in [commit 2997](http://wiki.powerdns.com/projects/trac/changeset/2997), closes [ticket 509](https://github.com/PowerDNS/pdns/issues/509).
-   Our DNS TXT parser is not 8-bit safe, but our DNS TXT writer assumes the reader is! Reported by Jan-Piet Mens in [ticket 541](https://github.com/PowerDNS/pdns/issues/541), [commit 2993](http://wiki.powerdns.com/projects/trac/changeset/2993) fixes our writer but not yet our parser.
-   Ruben d'Arco did some improvements to the MyDNS backend, and provided a full test suite for it, that we now run after every commit. Code in [commit 2988](http://wiki.powerdns.com/projects/trac/changeset/2988).
-   Some exceptions from backends would lose their meaning while bubbling up. Fixed by Aki Tuomi in [commit 2985](http://wiki.powerdns.com/projects/trac/changeset/2985), closing [ticket 639](https://github.com/PowerDNS/pdns/issues/639).
-   The packet-cache honours max reply length while matching cached packets against queries, but not EDNS status. This would mean that EDNS-enabled replies with a 512 reply len could be returned on non-EDNS queries. Spotted while investigating a report from Winfried Angele, patched by Ruben d'Arco in [commit 2982](http://wiki.powerdns.com/projects/trac/changeset/2982), closing [ticket 630](https://github.com/PowerDNS/pdns/issues/630).
-   Errors involving creating, deletion or changing permissions on the control socket were unclear. Ruben d'Arco improved this in [commit 2981](http://wiki.powerdns.com/projects/trac/changeset/2981).
-   pipe-timeout was always documented to be in milliseconds, but it turns out it was in seconds! [commit 2971](http://wiki.powerdns.com/projects/trac/changeset/2971) changes them to actually be in ms, and 'increases' the default from 1000 seconds to 2000 milliseconds.
-   Some exceptions would get dropped during inbound AXFR, yielding a log file that says 'transaction started' and nothing after that, making AXFR fail silently. [commit 2976](http://wiki.powerdns.com/projects/trac/changeset/2976) and [commit 2977](http://wiki.powerdns.com/projects/trac/changeset/2977) improve this somewhat.
-   We now error out on empty labels inside of names (www..example.com) instead of generating bogus reply packets. Code in [commit 2972](http://wiki.powerdns.com/projects/trac/changeset/2972), reported by several users.
-   Doing chmod before chown, instead of the other way around, apparently avoids requiring a whole SELinux capability. Reported by Sander Hoentjen, fixed in [commit 2965](http://wiki.powerdns.com/projects/trac/changeset/2965).
-   Christian Hofstaedtler fixed a bug in our Debian init.d script. Code in [commit 2963](http://wiki.powerdns.com/projects/trac/changeset/2963).
-   Superslave errors ('Unable to find backend willing to host ..') now include the NSset found at the master, to aid debugging. Code in [commit 2887](http://wiki.powerdns.com/projects/trac/changeset/2887).
-   [commit 2874](http://wiki.powerdns.com/projects/trac/changeset/2874) in RC1 broke compilation without SQLite3 and made query logging unreliable. Fixed in [commit 2888](http://wiki.powerdns.com/projects/trac/changeset/2888), [commit 2889](http://wiki.powerdns.com/projects/trac/changeset/2889).
-   The dnsreplay tool now processes single packet pcaps. Fix in [commit 2895](http://wiki.powerdns.com/projects/trac/changeset/2895).
-   PowerDNS always derives NSEC/NSEC3 from the actual zone content. To accommodate this, zone2sql now drops NSEC/NSEC3 records, as those should never be in a PowerDNS backend directly ([commit 2915](http://wiki.powerdns.com/projects/trac/changeset/2915)), bindbackend ignores NSEC/NSEC3 while reading zonefiles ([commit 2917](http://wiki.powerdns.com/projects/trac/changeset/2917)) and pdnssec reports NSEC/NSEC3 in the database as an error condition ([commit 2918](http://wiki.powerdns.com/projects/trac/changeset/2918)).
-   The bindbackend now ignores NSEC/NSEC3 records while reading zonefiles. Change in [commit 2917](http://wiki.powerdns.com/projects/trac/changeset/2917).
-   An EXPERIMENTAL feature ('direct-dnskey') for reading ZSKs from the records table/your BIND zonefile was added in [commit 2920](http://wiki.powerdns.com/projects/trac/changeset/2920), [commit 2921](http://wiki.powerdns.com/projects/trac/changeset/2921), [commit 2922](http://wiki.powerdns.com/projects/trac/changeset/2922).
-   While fully optional, PowerDNS supports direct RRSIG queries. Kees Monshouwer improved on our behaviour for those queries in [commit 2927](http://wiki.powerdns.com/projects/trac/changeset/2927).
-   IPv6 glue situations require AAAA records for the receiving end of a delegation in the ADDITIONAL section of a referral. This was supported ('do-ipv6-additional-processing') but not enabled by default. [commit 2929](http://wiki.powerdns.com/projects/trac/changeset/2929) enables it by default.
-   pdnssec check-zone now warns for CNAME-and-other data at names in your zones. Code by Ruben d'Arco in [commit 2930](http://wiki.powerdns.com/projects/trac/changeset/2930).
-   Positive ANY-responses would include a spurious NSEC3. Corrected in [commit 2932](http://wiki.powerdns.com/projects/trac/changeset/2932) and [commit 2933](http://wiki.powerdns.com/projects/trac/changeset/2933), cleaned up by Kees Monshouwer in [commit 2935](http://wiki.powerdns.com/projects/trac/changeset/2935).
-   The ldapbackend now allows overriding the base dn for AXFR subtree search. Fixed in [commit 2934](http://wiki.powerdns.com/projects/trac/changeset/2934), closing [ticket 536](https://github.com/PowerDNS/pdns/issues/536).

Changes below are in 3.2-RC1 and up.

## DNSSEC changes in 3.2
-   Kees Monshouwer did a tremendous amount of work to improve and perfect our DNSSEC implementation, mostly in the NSEC3 area. Code in [commit 2687](http://wiki.powerdns.com/projects/trac/changeset/2687), [commit 2689](http://wiki.powerdns.com/projects/trac/changeset/2689), [commit 2691](http://wiki.powerdns.com/projects/trac/changeset/2691), fixing [ticket 486](https://github.com/PowerDNS/pdns/issues/486), [ticket 537](https://github.com/PowerDNS/pdns/issues/537), [ticket 540](https://github.com/PowerDNS/pdns/issues/540). He also implemented support for Empty Non-Terminals, code in [commit 2721](http://wiki.powerdns.com/projects/trac/changeset/2721), [commit 2732](http://wiki.powerdns.com/projects/trac/changeset/2732), [commit 2745](http://wiki.powerdns.com/projects/trac/changeset/2745), fixing [ticket 127](https://github.com/PowerDNS/pdns/issues/127) and [ticket 558](https://github.com/PowerDNS/pdns/issues/558).
-   Presigned wildcard operation was improved with the help of many parties (see commit message for [commit 2676](http://wiki.powerdns.com/projects/trac/changeset/2676)). Presigned operation was also changed to be more consistent with master/live-signing operation. Code and a full test suite in [commit 2709](http://wiki.powerdns.com/projects/trac/changeset/2709), which also improves TTL behaviour for various situations. Fixes [ticket 460](https://github.com/PowerDNS/pdns/issues/460), [ticket 533](https://github.com/PowerDNS/pdns/issues/533), [ticket 559](https://github.com/PowerDNS/pdns/issues/559).
-   Depending on database & locale settings, names starting with underscore would sometimes cause broken records. [commit 2710](http://wiki.powerdns.com/projects/trac/changeset/2710) contains schema and code changes for the gpgsql and gmysql backends to sort this (no pun intended) definitively, closing [ticket 550](https://github.com/PowerDNS/pdns/issues/550). In addition, a pdnssec test-schema command was added (experimental and incomplete). It can be used to verify underscore sorting and a few other parameters of the database. Code in [commit 2714](http://wiki.powerdns.com/projects/trac/changeset/2714).
-   We now always include an EDNS section in responses to queries that also had an EDNS section. This was thought to improve BIND interoperability, but this turned out to be false. In any case, this change improves standards compliance. Spotted by Mats Dufberg, code in [commit 2649](http://wiki.powerdns.com/projects/trac/changeset/2649).
-   It turns out we were storing Botan keys the wrong way. Botan did not care but Polar did, causing interoperability problems. Fixed in [commit 2720](http://wiki.powerdns.com/projects/trac/changeset/2720), with the kind help of Paul Bakker of PolarSSL. Fixes [ticket 492](https://github.com/PowerDNS/pdns/issues/492) as reported by Florian Obser via Debian.
-   pdnssec add-zone-key now defaults to RSASHA256, like secure-zone already did. Code in [commit 2692](http://wiki.powerdns.com/projects/trac/changeset/2692).
-   pdns\_control purge now also purges DNSSEC-related caches (keys and metadata). Code in [commit 2694](http://wiki.powerdns.com/projects/trac/changeset/2694), by Ruben d'Arco. Fixes [ticket 530](https://github.com/PowerDNS/pdns/issues/530).
-   The signer thread would die in specific situations, leaving you with a non-working but very busy system. Fixed in [commit 2668](http://wiki.powerdns.com/projects/trac/changeset/2668), [commit 2670](http://wiki.powerdns.com/projects/trac/changeset/2670), closing [ticket 517](https://github.com/PowerDNS/pdns/issues/517).
-   pdnssec secure-zone now warns when you just signed a slave zone. Suggested by Mark Scholten, code in [commit 2795](http://wiki.powerdns.com/projects/trac/changeset/2795), closes [ticket 592](https://github.com/PowerDNS/pdns/issues/592).
-   pdnssec check-zone now warns about out-of-zone data. Patch by Kees Monshouwer in [commit 2826](http://wiki.powerdns.com/projects/trac/changeset/2826), closing [ticket 604](https://github.com/PowerDNS/pdns/issues/604).
-   pdnssec now honours --no-config. Patch by Kees Monshouwer in [commit 2810](http://wiki.powerdns.com/projects/trac/changeset/2810).
-   Various fixes for bindbackend presigned operation, mostly by Kees Monshouwer. Code in [commit 2815](http://wiki.powerdns.com/projects/trac/changeset/2815), closing [ticket 600](https://github.com/PowerDNS/pdns/issues/600).
-   Bindbackend could get confused about domain metadata, sometimes even causing hangs. Fixes by Kees Monshouwer in [commit 2819](http://wiki.powerdns.com/projects/trac/changeset/2819) and [commit 2834](http://wiki.powerdns.com/projects/trac/changeset/2834), closing [ticket 600](https://github.com/PowerDNS/pdns/issues/600) and [ticket 603](https://github.com/PowerDNS/pdns/issues/603).
-   SQL queries in gsql backends that reference the domain\_id column have been made explicit about from what table they want this column. This makes it easier to operate custom schemas without changing the queries. Fix by Nicky Gerritsen in [commit 2821](http://wiki.powerdns.com/projects/trac/changeset/2821).
-   In various situations involving CNAMEs and wildcards, and for ANY queries involving CNAMEs, we would sometimes return bogus results. Fixed in [commit 2825](http://wiki.powerdns.com/projects/trac/changeset/2825) by Kees Monshouwer.
-   rectify-zone accidentally set auth=1 on NS records of secure delegations. Reported by George Notaras, fixed by Kees Monshouwer in [commit 2831](http://wiki.powerdns.com/projects/trac/changeset/2831), closing [ticket 605](https://github.com/PowerDNS/pdns/issues/605).
-   The DNSSEC signature cache now actually gets cleaned up, avoiding lasting spikes in memory usage every thursday. Code in [commit 2836](http://wiki.powerdns.com/projects/trac/changeset/2836) and [commit 2843](http://wiki.powerdns.com/projects/trac/changeset/2843), closing [ticket 594](https://github.com/PowerDNS/pdns/issues/594).
-   Signatures used to roll at midnight on thursday. We now roll them one hour after midnight, with inception still set to midnight, to allow for some variations in clock quality on resolvers. Code in [commit 2857](http://wiki.powerdns.com/projects/trac/changeset/2857).
-   Duplicate records (same name/type/content/priority) would sometimes get broken RRSIGs during outgoing AXFR. Fixed in [commit 2856](http://wiki.powerdns.com/projects/trac/changeset/2856).
-   A root zone (name="") with DNSSEC would cause crashes in some situations. Reported by Luuk Hendriks. Fixed in [commit 2867](http://wiki.powerdns.com/projects/trac/changeset/2867), [commit 2868](http://wiki.powerdns.com/projects/trac/changeset/2868), closing [ticket 614](https://github.com/PowerDNS/pdns/issues/614).
-   Direct RRSIG queries for zones with auto-completed SOA records would cause trouble. Reported by Kees Monshouwer and fixed by him in [commit 2869](http://wiki.powerdns.com/projects/trac/changeset/2869).
-   When a name is matched only by a wildcard, but the type in the query is not present, we would be lacking one NSEC(3) record to prove the existence of the wildcard. Fixed by Kees Monshouwer in [commit 2872](http://wiki.powerdns.com/projects/trac/changeset/2872) and [commit 2873](http://wiki.powerdns.com/projects/trac/changeset/2873).
-   Luuk Hendriks spotted that our PolarSSL RSA key generation code was using inferior entropy. This can be important on virtual machines with badly implemented clocks. Fixed in [commit 2876](http://wiki.powerdns.com/projects/trac/changeset/2876), closing [ticket 615](https://github.com/PowerDNS/pdns/issues/615).

## Non-DNSSEC improvements/changes
-   Bindbackend would sometimes crash on startup, due to a sync\_with\_stdio call. This call has been moved to pdns\_server proper to occur before any threads are spawned, avoiding race conditions in this call. Note that this crash has only been observed twice in thousands of regression test runs and has never been reported in the real world. Change in [commit 2882](http://wiki.powerdns.com/projects/trac/changeset/2882).
-   Leen Besselink submitted query logging support for the SQLite3 parts in the bindbackend. Code in [commit 2874](http://wiki.powerdns.com/projects/trac/changeset/2874).
-   Multi-backend operation would sometimes cause garbage domain IDs to be passed to backends. Reported by Kees Monshouwer and fixed by him in [commit 2871](http://wiki.powerdns.com/projects/trac/changeset/2871).
-   Bindbackend would sometimes crash during reloads/rediscovers. The changes in [commit 2837](http://wiki.powerdns.com/projects/trac/changeset/2837) get rid of the crash, at the cost of returning SERVFAIL during reloads. Closes [ticket 564](https://github.com/PowerDNS/pdns/issues/564).
-   Our label decompression code was naive, causing troubles for slaving of very specifically formatted zones. Fix in [ticket 2822](https://github.com/PowerDNS/pdns/issues/2822), closes [ticket 599](https://github.com/PowerDNS/pdns/issues/599).
-   Bindbackend slaves would choke on unknown RR types and do silly things with RP and SRV records. Fixed in [commit 2811](http://wiki.powerdns.com/projects/trac/changeset/2811) and [commit 2812](http://wiki.powerdns.com/projects/trac/changeset/2812).
-   The luabackend can now compile against Lua 5.2. Patch by Fredrik Danerklint in [commit 2794](http://wiki.powerdns.com/projects/trac/changeset/2794), additional luabackend compile fixes in [commit 2854](http://wiki.powerdns.com/projects/trac/changeset/2854).
-   A new backend, the 'Remote backend' [Remote Backend](authoritative/backend-remote.md "Remote Backend") was submitted by Aki Tuomi. It aims to replace the pipebackend with a better protocol and support for more connection methods, including HTTP. Code in [commit 2755](http://wiki.powerdns.com/projects/trac/changeset/2755), [commit 2756](http://wiki.powerdns.com/projects/trac/changeset/2756), [commit 2757](http://wiki.powerdns.com/projects/trac/changeset/2757), [commit 2758](http://wiki.powerdns.com/projects/trac/changeset/2758), [commit 2759](http://wiki.powerdns.com/projects/trac/changeset/2759), [commit 2824](http://wiki.powerdns.com/projects/trac/changeset/2824), closing [ticket 529](https://github.com/PowerDNS/pdns/issues/529), [ticket 597](https://github.com/PowerDNS/pdns/issues/597).
-   The gsqlite (SQLite 2) backend was removed. We were not aware of any users and it was not actually working anyway. Changes in commits [2773](http://wiki.powerdns.com/projects/trac/changeset/2773)-[2777](http://wiki.powerdns.com/projects/trac/changeset/2777), closing [ticket 565](https://github.com/PowerDNS/pdns/issues/565).
-   Various tinydnsbackend improvements: ignore-bogus-records option; TAI offset updated; strip dots on names where suitable; various internal improvements. Code in [commit 2762](http://wiki.powerdns.com/projects/trac/changeset/2762).
-   gpgsql no longer logs the database password in connection errors. Code in [commit 2609](http://wiki.powerdns.com/projects/trac/changeset/2609), [commit 2612](http://wiki.powerdns.com/projects/trac/changeset/2612), closing [ticket 459](https://github.com/PowerDNS/pdns/issues/459).
-   You can now finally specify 0.0.0.0 or :: as local-address/local-ipv6 without getting replies from the wrong address. This much-requested feature is implemented in [commit 2763](http://wiki.powerdns.com/projects/trac/changeset/2763), [commit 2766](http://wiki.powerdns.com/projects/trac/changeset/2766), [commit 2779](http://wiki.powerdns.com/projects/trac/changeset/2779) and [commit 2781](http://wiki.powerdns.com/projects/trac/changeset/2781). Tested on Linux, FreeBSD and Mac OS X.
-   3.2 can be reliably built with or without Lua. This and many other configure/compile-related fixes in [commit 2610](http://wiki.powerdns.com/projects/trac/changeset/2610), [commit 2611](http://wiki.powerdns.com/projects/trac/changeset/2611) / [ticket 461](https://github.com/PowerDNS/pdns/issues/461), [commit 2666](http://wiki.powerdns.com/projects/trac/changeset/2666), [commit 2671](http://wiki.powerdns.com/projects/trac/changeset/2671), [commit 2672](http://wiki.powerdns.com/projects/trac/changeset/2672) / [ticket 522](https://github.com/PowerDNS/pdns/issues/522), [commit 2673](http://wiki.powerdns.com/projects/trac/changeset/2673) / [ticket 522](https://github.com/PowerDNS/pdns/issues/522), [commit 2696](http://wiki.powerdns.com/projects/trac/changeset/2696) / [ticket 555](https://github.com/PowerDNS/pdns/issues/555), [commit 2697](http://wiki.powerdns.com/projects/trac/changeset/2697) / [ticket 457](https://github.com/PowerDNS/pdns/issues/457), [commit 2698](http://wiki.powerdns.com/projects/trac/changeset/2698), [commit 2708](http://wiki.powerdns.com/projects/trac/changeset/2708), [commit 2742](http://wiki.powerdns.com/projects/trac/changeset/2742) / [ticket 462](https://github.com/PowerDNS/pdns/issues/462)), [commit 2752](http://wiki.powerdns.com/projects/trac/changeset/2752) / [ticket 437](https://github.com/PowerDNS/pdns/issues/437), [commit 2764](http://wiki.powerdns.com/projects/trac/changeset/2764), [commit 2809](http://wiki.powerdns.com/projects/trac/changeset/2809), [commit 2844](http://wiki.powerdns.com/projects/trac/changeset/2844), [commit 2845](http://wiki.powerdns.com/projects/trac/changeset/2845), [commit 2846](http://wiki.powerdns.com/projects/trac/changeset/2846), [commit 2881](http://wiki.powerdns.com/projects/trac/changeset/2881).
-   Juraj Lutter contributed AXFR-SOURCE per zone metadata settings. Code in [commit 2616](http://wiki.powerdns.com/projects/trac/changeset/2616).
-   Initscripts now have exit codes, submitted by Sander Hoentjen. Code in [commit 2728](http://wiki.powerdns.com/projects/trac/changeset/2728). Guardian now returns 0 instead of 1 when receiving SIGTERM, requested by Morten Stevens of Fedora. Code in [commit 2717](http://wiki.powerdns.com/projects/trac/changeset/2717).
-   Mark Zealey submitted various performance improvement patches and suggestions. Accepted as [commit 2729](http://wiki.powerdns.com/projects/trac/changeset/2729) / [ticket 579](https://github.com/PowerDNS/pdns/issues/579), [commit 2730](http://wiki.powerdns.com/projects/trac/changeset/2730) / [ticket 584](https://github.com/PowerDNS/pdns/issues/584)), [commit 2731](http://wiki.powerdns.com/projects/trac/changeset/2731) / [ticket 583](https://github.com/PowerDNS/pdns/issues/583)), [commit 2768](http://wiki.powerdns.com/projects/trac/changeset/2768) / [ticket 578](https://github.com/PowerDNS/pdns/issues/578)). Please see commit messages for more details.
-   pdnssec check-all-zones now reuses database connections, avoiding a socket exhaustion issue in some situations. Code in [commit 2749](http://wiki.powerdns.com/projects/trac/changeset/2749), closes [ticket 519](https://github.com/PowerDNS/pdns/issues/519).
-   Ruben d'Arco submitted various improvements regarding trailing dots. Additional lookups now try harder, pdnssec errors about trailing dots in names, pdnssec warns about trailing dots in names inside content fields, AXFR now strips the dot from SRV hostnames. Code in [commit 2748](http://wiki.powerdns.com/projects/trac/changeset/2748), fixes [ticket 289](https://github.com/PowerDNS/pdns/issues/289).
-   Pre-3.0, backends would get cycled if they threw the right error. 3.2 reinstates this behaviour, as it is more robust. Change in [commit 2734](http://wiki.powerdns.com/projects/trac/changeset/2734) (reverting [commit 2100](http://wiki.powerdns.com/projects/trac/changeset/2100)), fixes [ticket 386](https://github.com/PowerDNS/pdns/issues/386).
-   PowerDNS auth does not use the select() kernel/library call anymore. This means fd-numbers over 1023 (and, in general, more than 1024 sockets, including more than 1024 listening sockets) should now work reliably. Code in [commit 2739](http://wiki.powerdns.com/projects/trac/changeset/2739), [commit 2740](http://wiki.powerdns.com/projects/trac/changeset/2740), fixes [ticket 408](https://github.com/PowerDNS/pdns/issues/408).
-   gmysql users can now specify the 'group' we connect as, using the gmysql-group setting. Submitted by Kees Monshouwer, code in [commit 2770](http://wiki.powerdns.com/projects/trac/changeset/2770), [commit 2771](http://wiki.powerdns.com/projects/trac/changeset/2771), [commit 2778](http://wiki.powerdns.com/projects/trac/changeset/2778), [commit 2780](http://wiki.powerdns.com/projects/trac/changeset/2780), closing [ticket 463](https://github.com/PowerDNS/pdns/issues/463).
-   The Linux-only traceback handler is now optional (use traceback-handler=off to disable it). Suggested by Marc Haber. Change in [commit 2798](http://wiki.powerdns.com/projects/trac/changeset/2798), closes [ticket 497](https://github.com/PowerDNS/pdns/issues/497).
-   We now use IPV6\_V6ONLY to bind IPv6 sockets. This ensures consistent behaviour between different operating systems. Change in [commit 2799](http://wiki.powerdns.com/projects/trac/changeset/2799).
-   MySQL connections are now logged at a higher loglevel, reducing log clutter. Change in [commit 2800](http://wiki.powerdns.com/projects/trac/changeset/2800).
-   We now ship a systemd unit file in contrib/. Added in [commit 2847](http://wiki.powerdns.com/projects/trac/changeset/2847) and [commit 2848](http://wiki.powerdns.com/projects/trac/changeset/2848), submitted by Morten Stevens.

## Assorted bugfixes
-   If a slave domain is removed while a transfer for it is queued, we no longer try the transfer. This also avoids a rare crash in similar circumstances. Code in [commit 2802](http://wiki.powerdns.com/projects/trac/changeset/2802), closes [ticket 596](https://github.com/PowerDNS/pdns/issues/596).
-   When using pdnssec with gsql backends, sometimes an SSqlException would pop up without any useful information. This no longer happens and errors are now in general more meaningful. Fix in [commit 2803](http://wiki.powerdns.com/projects/trac/changeset/2803).
-   zone2sql now uses correct string syntax for PostgreSQL. This is needed for importing with the changed default settings in PostgreSQL 9.2 and up. Code in [commit 2797](http://wiki.powerdns.com/projects/trac/changeset/2797), closes [ticket 471](https://github.com/PowerDNS/pdns/issues/471).
-   We no longer send v6 notifications if v6 is not available. Same for IPv4. Code in [commit 2772](http://wiki.powerdns.com/projects/trac/changeset/2772), fixes [ticket 515](https://github.com/PowerDNS/pdns/issues/515).
-   We would sometimes serve stale data after an incoming AXFR. Reported by Martin Draschl, fixed by Ruben d'Arco in [commit 2699](http://wiki.powerdns.com/projects/trac/changeset/2699), closing [ticket 525](https://github.com/PowerDNS/pdns/issues/525).
-   Duplicate incoming NOTIFYs could cause PowerDNS to try to insert the same domain name into a database twice. Fixed in [commit 2703](http://wiki.powerdns.com/projects/trac/changeset/2703), closing [ticket 453](https://github.com/PowerDNS/pdns/issues/453).
-   pdnssec show-zone now works on a zone that has any number of keys, instead of requiring active keys. Reported by Jeroen Tushuizen of myH2Oservers, code in [commit 2769](http://wiki.powerdns.com/projects/trac/changeset/2769), closes [ticket 586](https://github.com/PowerDNS/pdns/issues/586).
-   pdns-control notify-host now accepts v6 literals. Reported by Christof Meerwald, fixed in [commit 2704](http://wiki.powerdns.com/projects/trac/changeset/2704).
-   The tinydnsbackend no longer chokes on questions longer than 64 bytes. Code in [commit 2622](http://wiki.powerdns.com/projects/trac/changeset/2622).
-   *-all-domains commands in pdnssec now work with Postgres (gpgsql) too. Code in [commit 2645](http://wiki.powerdns.com/projects/trac/changeset/2645), closing [ticket 472](https://github.com/PowerDNS/pdns/issues/472).
-   We would sometimes leave the opcode of an outgoing packet uninitialized. Fixed in [commit 2680](http://wiki.powerdns.com/projects/trac/changeset/2680), closing [ticket 532](https://github.com/PowerDNS/pdns/issues/532).
-   nproxy can now listen on a configurable port. Code in [commit 2684](http://wiki.powerdns.com/projects/trac/changeset/2684), fixes [ticket 534](https://github.com/PowerDNS/pdns/issues/534).
-   Improve mydnsbackend for SOA queries. Code in [commit 2751](http://wiki.powerdns.com/projects/trac/changeset/2751), fixes [ticket 439](https://github.com/PowerDNS/pdns/issues/439), by Ruben d'Arco.
-   Various non-functional fixes that make Valgrind happy (note that Valgrind was right to complain in all of these situations), in [commit 2715](http://wiki.powerdns.com/projects/trac/changeset/2715), [commit 2716](http://wiki.powerdns.com/projects/trac/changeset/2716), [commit 2718](http://wiki.powerdns.com/projects/trac/changeset/2718).

# PowerDNS Authoritative Server 3.1
Released on the 4th of May 2012
RC3 released on the 30th of April 2012
RC2 released on the 14th of April 2012
RC1 released on the 23th of March 2012

**Warning**: Version 3.1 of the PowerDNS Authoritative Server is a major upgrade if you are coming from 2.9.x. There are also some important changes if you are coming from 3.0. Please refer to the [Upgrade documentation](authoritative/upgrading.md) for important information on correct and stable operation, as well as notes on performance and memory use.

Version 3.1 of the PowerDNS Authoritative Server represents the 'coming of age' of our DNSSEC implementation. In addition, 3.1 solves a lot of '.0' issues typically associated with a major new release.

As usual, we are very grateful for the involvement of the PowerDNS community. The uptake of 3.0 was rapid, and many users were very helpful in shaking out the bugs, and willing to test the fixes we provided or, in many cases, provided the fixes themselves.

Of specific note is the giant PowerDNS DNSSEC deployment in Sweden by Atomia and Binero. PowerDNS 3.0 now powers over 150000 DNSSEC domains in Sweden, around 95% of all DNSSEC domains, in a country were most internet service providers actually validate all .SE domains.

Finally, this release has benefited a lot from Peter van Dijk joining us, as he has merged a tremendous amount of patches, cleaned up years of accumulated dust in the code, and massively improved our regression testing into a full blown continuous integration setup with full DNSSEC tests!

Additionally, we would like to thank Ruben d'Arco, Jose Arthur Benetasso Villanova, Marc Haber, Jimmy Bergman, Aki Tuomi and everyone else who helped us out!

## Downloads
-   [Official download page](http://www.powerdns.com/content/downloads.html)
-   [CentOS/RHEL 5/6 RPMs](http://www.monshouwer.eu/download/3rd_party/pdns-server/) kindly provided by Kees Monshouwer.
-   [Additional packages](http://wiki.powerdns.com/trac#GettingPowerDNSpackages) kindly provided by various other people.

## Changes between RC3 and final
-   pdnssec now honours the default-soa-name setting. Reported by Kees Monshouwer, fixed in [commit 2600](http://wiki.powerdns.com/projects/trac/changeset/2600).

## Changes between RC2 and RC3
-   The hidden test-algorithms command for pdnssec now has a little brother 'test-algorithm X'. Code in [commit 2596](http://wiki.powerdns.com/projects/trac/changeset/2596), by Aki Tuomi.
-   PolarSSL upgraded to 1.1.2 due to weak RSA key generation ([commit 2586](http://wiki.powerdns.com/projects/trac/changeset/2586)). If you created RSA keys with RC1 or RC2 using PolarSSL, please replace them! This upgrade introduced a slowdown; speedup patch in [commit 2593](http://wiki.powerdns.com/projects/trac/changeset/2593).
-   It turns out we were using libmysqlclient in a thread-unsafe manner. This issue was reported and painstakingly debugged by Marc Haber. Presumably fixed in [commit 2591](http://wiki.powerdns.com/projects/trac/changeset/2591).
-   Updated a bunch of internal counters to be threadsafe. Code in [commit 2579](http://wiki.powerdns.com/projects/trac/changeset/2579).
-   NSEC(3) bitmaps can now cover RRtypes above 255. Reported by Michael Braunoeder, patch by Aki Tuomi in [commit 2590](http://wiki.powerdns.com/projects/trac/changeset/2590).
-   pdnssec check-zone now reports MBOXFW and URL records (as those are unsupported since 3.0). Reported by Gerwin Krist of Digitalus, patch by Ruben d'Arco. Closes [ticket 446](https://github.com/PowerDNS/pdns/issues/446).
-   The odbcbackend was removed. It only runs on Windows and Windows is unsupported since 3.0. Removal in [commit 2576](http://wiki.powerdns.com/projects/trac/changeset/2576).
-   We used to send the chunk length and the actual chunk in two separate writes (often resulting in two separate TCP packets) during outbound AXFR. This confused MSDNS. We now combine those writes. Code in [commit 2575](http://wiki.powerdns.com/projects/trac/changeset/2575).
-   The bindbackend can now run without SQLite3, as previously intended. Fix in [commit 2574](http://wiki.powerdns.com/projects/trac/changeset/2574).
-   Some high-concurrency master setups would crash under load. Fixed in [commit 2571](http://wiki.powerdns.com/projects/trac/changeset/2571).

# Changes between RC1 and RC2
-   We imported the TinyDNS backend by Ruben d'Arco. Code mostly in [commit 2559](http://wiki.powerdns.com/projects/trac/changeset/2559). See [TinyDNS Backend](authoritative/backend-tinydns.md "TinyDNS Backend").
-   Overriding C(XX)FLAGS is easier now. Problem pointed out by Jose Arthur Benetasso Villanova and others, fix suggested by Sten Spans. Patch in [commit 2533](http://wiki.powerdns.com/projects/trac/changeset/2533).
-   TSIG fixes: skip embedded spaces in keys ([commit 2536](http://wiki.powerdns.com/projects/trac/changeset/2536)), compute signatures correctly (by Ruben d'Arco in [commit 2547](http://wiki.powerdns.com/projects/trac/changeset/2547)),
-   nproxy, dnsscan and dnsdemog did not compile at all. Fixes in [commit 2538](http://wiki.powerdns.com/projects/trac/changeset/2538), [commit 2554](http://wiki.powerdns.com/projects/trac/changeset/2554).
-   We now allow unescaped tabs in TXT records. Fix in [commit 2539](http://wiki.powerdns.com/projects/trac/changeset/2539).
-   SOA records no longer disappear during incoming transfers. Fix by Ruben d'Arco in [commit 2540](http://wiki.powerdns.com/projects/trac/changeset/2540).
-   PowerDNS compiles on OS X (and other platforms that support our auth server but not the recursor) again, fix in [commit 2566](http://wiki.powerdns.com/projects/trac/changeset/2566).
-   Cleanups related to warnings from gcc and valgrind in [commit 2561](http://wiki.powerdns.com/projects/trac/changeset/2561), [commit 2562](http://wiki.powerdns.com/projects/trac/changeset/2562), [commit 2565](http://wiki.powerdns.com/projects/trac/changeset/2565).
-   Solaris compatibility fixes by Ruben d'Arco, Juraj Lutter and others in [commit 2548](http://wiki.powerdns.com/projects/trac/changeset/2548), [commit 2552](http://wiki.powerdns.com/projects/trac/changeset/2552), [commit 2553](http://wiki.powerdns.com/projects/trac/changeset/2553), [commit 2560](http://wiki.powerdns.com/projects/trac/changeset/2560). Fixes for *BSD in [commit 2546](http://wiki.powerdns.com/projects/trac/changeset/2546).
-   pdns\_control help would report 'version' twice, reported by Gerwin, fix in [commit 2549](http://wiki.powerdns.com/projects/trac/changeset/2549).

## DNSSEC related fixes
-   When slaving zones, PowerDNS now automatically detects that a zone is presigned. Code in [commit 2502](http://wiki.powerdns.com/projects/trac/changeset/2502), closing [ticket 369](https://github.com/PowerDNS/pdns/issues/369), [ticket 392](https://github.com/PowerDNS/pdns/issues/392).
-   The bindbackend can now manage its own SQLite3 database to store key data, removing the need to run it with a gsql backend. Code in [commit 2448](http://wiki.powerdns.com/projects/trac/changeset/2448), [commit 2449](http://wiki.powerdns.com/projects/trac/changeset/2449), [commit 2450](http://wiki.powerdns.com/projects/trac/changeset/2450), [commit 2451](http://wiki.powerdns.com/projects/trac/changeset/2451), [commit 2452](http://wiki.powerdns.com/projects/trac/changeset/2452), [commit 2453](http://wiki.powerdns.com/projects/trac/changeset/2453), [commit 2455](http://wiki.powerdns.com/projects/trac/changeset/2455), [commit 2482](http://wiki.powerdns.com/projects/trac/changeset/2482), [commit 2496](http://wiki.powerdns.com/projects/trac/changeset/2496), [commit 2499](http://wiki.powerdns.com/projects/trac/changeset/2499).
-   NSEC/NSEC3 logic for picking 'boundary' names was tricky, and got it wrong in some cases. Fixes in [commit 2289](http://wiki.powerdns.com/projects/trac/changeset/2289), [commit 2429](http://wiki.powerdns.com/projects/trac/changeset/2429), [commit 2435](http://wiki.powerdns.com/projects/trac/changeset/2435) and [commit 2473](http://wiki.powerdns.com/projects/trac/changeset/2473).
-   The subtle differences between 'what records get NSEC', 'what records get NSEC3' and 'what records should get signed' did not translate well to the SQL auth column. We now use 'ordername IS NULL' to map the whole spectrum. Code in [commit 2477](http://wiki.powerdns.com/projects/trac/changeset/2477), [commit 2480](http://wiki.powerdns.com/projects/trac/changeset/2480), [commit 2492](http://wiki.powerdns.com/projects/trac/changeset/2492).
-   Pre-signed AXFR output, although correct, was different from our query responses. Rectified in [commit 2477](http://wiki.powerdns.com/projects/trac/changeset/2477).
-   Spotted & fixed by Jimmy Bergman of Atomia, CNAMEs and RRSIGs could have bad interactions. Fix in [commit 2314](http://wiki.powerdns.com/projects/trac/changeset/2314), further refined in [commit 2318](http://wiki.powerdns.com/projects/trac/changeset/2318). Closes [ticket 411](https://github.com/PowerDNS/pdns/issues/411).
-   Spotted & fixed by Jimmy Bergman of Atomia, we now allow direct RRSIG queries even when do=0.
-   Spotted by Mark Scholten and Marco Davids, we would sometimes generate duplicate (and wrong) RRSIGs when signing an ANY answer because of record jumbling. Fix in [commit 2381](http://wiki.powerdns.com/projects/trac/changeset/2381).
-   Several fixes to handling of DS queries, in [commit 2420](http://wiki.powerdns.com/projects/trac/changeset/2420), [commit 2510](http://wiki.powerdns.com/projects/trac/changeset/2510), [commit 2512](http://wiki.powerdns.com/projects/trac/changeset/2512).
-   We now lowercase the signer name in an RRSIG. This is not mandated by DNSSEC specification but it improves compatibility with some validators. Fix in [commit 2426](http://wiki.powerdns.com/projects/trac/changeset/2426).

## Bug fixes
-   Winfried Angele discovered we would open an additional backend connection per zone in the BIND backend. This only impacted users with multiple simultaneous backends. Fix in [commit 2253](http://wiki.powerdns.com/projects/trac/changeset/2253), closing [ticket 383](https://github.com/PowerDNS/pdns/issues/383).
-   All versions of max-cache-entries setting had confusing behaviour when set to 0. Now clarified to mean that 0 truly means 0, and not 'infinite'. Change in [commit 2328](http://wiki.powerdns.com/projects/trac/changeset/2328).
-   Wildcards in the presence of delegations were broken. Reported by a cast of thousands. Fix & regression test in [commit 2368](http://wiki.powerdns.com/projects/trac/changeset/2368). Closes [ticket 389](https://github.com/PowerDNS/pdns/issues/389).
-   Internal caches used an order of magnitude more memory than expected and some were not purged properly, which hindered real life deployments. Spotted by Winfried Angele and others. Fixed in [commit 2287](http://wiki.powerdns.com/projects/trac/changeset/2287) and [commit 2328](http://wiki.powerdns.com/projects/trac/changeset/2328).
-   Christof Meerwald discovered our .tar file missed a file of the Lua backend. Change in [commit 2257](http://wiki.powerdns.com/projects/trac/changeset/2257).
-   Paul Xek found out that the edns-subnet support did not work for subnets tinier than a /25 or /121. Fix in [commit 2258](http://wiki.powerdns.com/projects/trac/changeset/2258).
-   edns-subnet aware PIPE scripts received bogus remote information on AXFR requests. Fixed in [commit 2284](http://wiki.powerdns.com/projects/trac/changeset/2284).
-   Fix compilation against older versions of MySQL that do not have MYSQL\_OPT\_RECONNECT. [commit 2264](http://wiki.powerdns.com/projects/trac/changeset/2264), closing [ticket 378](https://github.com/PowerDNS/pdns/issues/378).
-   D. Stussy of Snarked.net discovered that PowerDNS could not parse a DNS packet with a trailing blob of unknown length. Fixed in [commit 2267](http://wiki.powerdns.com/projects/trac/changeset/2267).
-   'pdnssec' did not work for records with NULL ttls. Fixed in [commit 2266](http://wiki.powerdns.com/projects/trac/changeset/2266), closing [ticket 432](https://github.com/PowerDNS/pdns/issues/432).
-   Pipe backend had issues parsing IPv6 records in ABI version 3. Fixed in [commit 2260](http://wiki.powerdns.com/projects/trac/changeset/2260).
-   We truncated the altitude in LOC records! I hope no one got lost. Fix in [commit 2268](http://wiki.powerdns.com/projects/trac/changeset/2268).
-   Xander Soldaat discovered that even if the web server was not configured, we'd still listen on the port. Fix in [commit 2269](http://wiki.powerdns.com/projects/trac/changeset/2269), closes [ticket 402](https://github.com/PowerDNS/pdns/issues/402).
-   The PIPE backend issues frequent fork()s, leading to potential fd leaks if these are not marked as 'close on exec'. Solved in [commit 2273](http://wiki.powerdns.com/projects/trac/changeset/2273), closing [ticket 194](https://github.com/PowerDNS/pdns/issues/194).
-   Robert van der Meulen found that we messed up the interaction between wildcards and CNAMEs. Fixed in [commit 2276](http://wiki.powerdns.com/projects/trac/changeset/2276), which also adds a regression test to prevent this issue from recurring.
-   Fred Wittekind discovered that our notification proxy 'nproxy' no longer built from source. Fixed in [commit 2278](http://wiki.powerdns.com/projects/trac/changeset/2278).
-   Grant Keller found that we were inconsistent with spaces in labels, thus breaking DNS-SD. Fix in [commit 2305](http://wiki.powerdns.com/projects/trac/changeset/2305).
-   Winfried Angele fixed our autoconf script for Lua detection in [commit 2308](http://wiki.powerdns.com/projects/trac/changeset/2308).
-   BIND backend would leak an fd when including a configuration file from named.conf. Spotted by Hannu Ylitalo of Nebula Oy in [commit 2359](http://wiki.powerdns.com/projects/trac/changeset/2359).
-   GSQLite3 backend could crash on a network error at the wrong moment, leading to a restart by the guardian. Fix in [commit 2336](http://wiki.powerdns.com/projects/trac/changeset/2336).
-   './configure --enable-verbose-logging' was broken, fixed in [commit 2312](http://wiki.powerdns.com/projects/trac/changeset/2312).
-   PowerDNS would serve up old SOA data immediately after sending out a notification. Complicated bug documented perfectly in [ticket 427](https://github.com/PowerDNS/pdns/issues/427), which also came with not one but with two different patches to fix the problem. Thanks to Keith Buck. Code in [commit 2408](http://wiki.powerdns.com/projects/trac/changeset/2408).
-   Flag '--start-id' in zone2sql was not functional. Removed for now in [commit 2387](http://wiki.powerdns.com/projects/trac/changeset/2387), closing [ticket 332](https://github.com/PowerDNS/pdns/issues/332).
-   Our distribution tarball did not have the SQL schemas. Fixed in [commit 2459](http://wiki.powerdns.com/projects/trac/changeset/2459) and [commit 2460](http://wiki.powerdns.com/projects/trac/changeset/2460).
-   "Empty" MX records would confuse one of our parsers. Fixed in [commit 2468](http://wiki.powerdns.com/projects/trac/changeset/2468), closing Debian bug 533023.
-   The pdns.conf 'wildcards'-setting did not do anything in 3.0, so it was removed. Change in [commit 2508](http://wiki.powerdns.com/projects/trac/changeset/2508), [commit 2509](http://wiki.powerdns.com/projects/trac/changeset/2509).
-   Additional processing based on records loaded by the BIND backend might fail because of a trailing dot mismatch. Fix in [commit 2398](http://wiki.powerdns.com/projects/trac/changeset/2398).

## New features
-   Per-zone AXFR ACLs, based on the allow-axfr-ips zone metadata item. Code in [commit 2274](http://wiki.powerdns.com/projects/trac/changeset/2274). Also, remove some remains of our previous approach to supporting this in [commit 2326](http://wiki.powerdns.com/projects/trac/changeset/2326).
-   New SOA Serial Tweak mode INCEPTION-EPOCH for when operating as a 'signing slave', contributed by Jimmy Bergman. Code and documentation in [commit 2320](http://wiki.powerdns.com/projects/trac/changeset/2320).
-   Newlines in the 'content' field of backends are now allowed, restoring some DKIM setups to working condition. Update in [commit 2394](http://wiki.powerdns.com/projects/trac/changeset/2394), closing [ticket 395](https://github.com/PowerDNS/pdns/issues/395).

## Improvements
-   Depending on the encoding used, MySQL could take issue with our 'tsigkeys' table which contained very large rows. Trimmed in [commit 2400](http://wiki.powerdns.com/projects/trac/changeset/2400), closing [ticket 410](https://github.com/PowerDNS/pdns/issues/410).
-   Various build/configure-related fixes in [commit 2319](http://wiki.powerdns.com/projects/trac/changeset/2319), [commit 2373](http://wiki.powerdns.com/projects/trac/changeset/2373), [commit 2386](http://wiki.powerdns.com/projects/trac/changeset/2386), closing [ticket 380](https://github.com/PowerDNS/pdns/issues/380), [ticket 405](https://github.com/PowerDNS/pdns/issues/405), [ticket 420](https://github.com/PowerDNS/pdns/issues/420).
-   We now show the SOA serial after zone transfers. Code in [commit 2385](http://wiki.powerdns.com/projects/trac/changeset/2385), closing [ticket 416](https://github.com/PowerDNS/pdns/issues/416).
-   Ruben d'Arco submitted a full rework of our slave-side AXFR TSIG handling, closing [ticket 393](https://github.com/PowerDNS/pdns/issues/393) and [ticket 400](https://github.com/PowerDNS/pdns/issues/400) in the process. Code in [commit 2506](http://wiki.powerdns.com/projects/trac/changeset/2506). Additional improvement in [commit 2513](http://wiki.powerdns.com/projects/trac/changeset/2513).
-   The records.name-column in the gpgsql schema is now constrained to lowercase, as PowerDNS would be unable to find other entries anyway. Fix in [commit 2503](http://wiki.powerdns.com/projects/trac/changeset/2503), closing [ticket 426](https://github.com/PowerDNS/pdns/issues/426).
-   The gsql-backends can now handle huge records, thanks to a patch by Ruben d'Arco. Code in [commit 2476](http://wiki.powerdns.com/projects/trac/changeset/2476), closing [ticket 407](https://github.com/PowerDNS/pdns/issues/407). Additional changes in [commit 2292](http://wiki.powerdns.com/projects/trac/changeset/2292), [commit 2487](http://wiki.powerdns.com/projects/trac/changeset/2487), [commit 2489](http://wiki.powerdns.com/projects/trac/changeset/2489). Closes [ticket 218](https://github.com/PowerDNS/pdns/issues/218), [ticket 316](https://github.com/PowerDNS/pdns/issues/316).
-   Some of PowerDNS' internal classes would work with uninitialized data when repurposed outside of the PowerDNS core logic. Fix in [commit 2469](http://wiki.powerdns.com/projects/trac/changeset/2469),
-   pdnssec now has 'check-all-zones' and 'rectify-all-zones' commands. Submitted by Ruben d'Arco, code in [commit 2467](http://wiki.powerdns.com/projects/trac/changeset/2467).
-   'restart' in our init.d-script would not start pdns if it was down before. Fixed in [commit 2462](http://wiki.powerdns.com/projects/trac/changeset/2462).
-   'pdnssec rectify-zone' now honours --verbose and is rather quiet without it. Code in [commit 2443](http://wiki.powerdns.com/projects/trac/changeset/2443).
-   Improved error messages for systems without IPv6. Changes in [commit 2425](http://wiki.powerdns.com/projects/trac/changeset/2425).
-   The packet- and querycache now honour TTLs from backend data. Code in [commit 2414](http://wiki.powerdns.com/projects/trac/changeset/2414).
-   'pdns\_control help' now shows useful usage information. Code in [commit 2410](http://wiki.powerdns.com/projects/trac/changeset/2410) and [commit 2465](http://wiki.powerdns.com/projects/trac/changeset/2465).
-   Jasper Spaans improved our init.d script for compliance with Debian Squeeze. Patch in [commit 2251](http://wiki.powerdns.com/projects/trac/changeset/2251). Further improvement with 'set -e' to initscript contributed by Marc Haber in [commit 2301](http://wiki.powerdns.com/projects/trac/changeset/2301).
-   Klaus Darilion discovered our configuration file template and --help output explained the various cache TTLs wrongly, and he also added documentation for some missing parameters. [commit 2271](http://wiki.powerdns.com/projects/trac/changeset/2271) and [commit 2272](http://wiki.powerdns.com/projects/trac/changeset/2272).
-   Add support for building against Botan 1.10 (stable) and drop support for 1.9 (development). Changes in [commit 2334](http://wiki.powerdns.com/projects/trac/changeset/2334). This fixes several bugs when building against 1.9.
-   Upgrade internal PolarSSL library to their version 1.1.1. Change in [commit 2389](http://wiki.powerdns.com/projects/trac/changeset/2389) and beyond.
-   Compilation of several backends failed for Boost in non-standard locations. Fixes in [commit 2316](http://wiki.powerdns.com/projects/trac/changeset/2316)..
-   We now do additional processing for SRV records too. Code in [commit 2388](http://wiki.powerdns.com/projects/trac/changeset/2388), closing [ticket 423](https://github.com/PowerDNS/pdns/issues/423) (which also contained the patch). Regression test updates that flow from this in [commit 2390](http://wiki.powerdns.com/projects/trac/changeset/2390).
-   Fix compilation on OSX. [commit 2316](http://wiki.powerdns.com/projects/trac/changeset/2316).
-   Fix pdnssec crash when asked to do DNSSEC without a DNSSEC capable backend. Code in [commit 2369](http://wiki.powerdns.com/projects/trac/changeset/2369).
-   If PowerDNS was not configured to operate as a DNS master, it would still accept 'pdns\_control notify' commands, but then not do it. Spotted by David Gavarret, patch by Jose Arthur Benetasso Villanova in [commit 2379](http://wiki.powerdns.com/projects/trac/changeset/2379).
-   In various places we would only accept UPPERCASE DNS typenames. Fixed in [commit 2370](http://wiki.powerdns.com/projects/trac/changeset/2370), closing [ticket 390](https://github.com/PowerDNS/pdns/issues/390).
-   We would not always drop supplemental groups correctly. Reported by David Black of Atlassian.
-   Our regression tests have been strengthened a lot, and now cover way more features. Commits in [2280](http://wiki.powerdns.com/projects/trac/changeset/2280), [2281](http://wiki.powerdns.com/projects/trac/changeset/2281), [2282](http://wiki.powerdns.com/projects/trac/changeset/2282), [2317](http://wiki.powerdns.com/projects/trac/changeset/2317), [2348](http://wiki.powerdns.com/projects/trac/changeset/2348), [2349](http://wiki.powerdns.com/projects/trac/changeset/2349), [2350](http://wiki.powerdns.com/projects/trac/changeset/2350), [2351](http://wiki.powerdns.com/projects/trac/changeset/2351) and beyond.
-   Update to support the latest draft of DANE/TLSA. Spotted by James Cloos ([commit 2338](http://wiki.powerdns.com/projects/trac/changeset/2338)). Further improvements by Pieter Lexis in [commit 2347](http://wiki.powerdns.com/projects/trac/changeset/2347), [commit 2358](http://wiki.powerdns.com/projects/trac/changeset/2358).
-   Compilation on OpenBSD was eased by patches from Brad Smith, which can be found in [commit 2288](http://wiki.powerdns.com/projects/trac/changeset/2288) and [commit 2291](http://wiki.powerdns.com/projects/trac/changeset/2291), closing [ticket 95](https://github.com/PowerDNS/pdns/issues/95).
-   'make check' failed on the internal PolarSSL. Spotted by Daniel Briley, fix in [commit 2283](http://wiki.powerdns.com/projects/trac/changeset/2283).
-   The default SQL schemas were expanded to contain far longer content fields. [commit 2292](http://wiki.powerdns.com/projects/trac/changeset/2292), [commit 2293](http://wiki.powerdns.com/projects/trac/changeset/2293).
-   Documentation typos, Jake Spencer ([commit 2304](http://wiki.powerdns.com/projects/trac/changeset/2304)), Jose Arthur Benetasso Villanova ([commit 2337](http://wiki.powerdns.com/projects/trac/changeset/2337)). Code typos in [commit 2324](http://wiki.powerdns.com/projects/trac/changeset/2324) (closes [ticket 296](https://github.com/PowerDNS/pdns/issues/296)).
-   Manpage updates from Debian, provided by Matthijs Möhlmann. Content in [commit 2306](http://wiki.powerdns.com/projects/trac/changeset/2306).
-   pdnssec rectify-zone can now accept multiple zones at the same time. Code in [commit 2383](http://wiki.powerdns.com/projects/trac/changeset/2383).
-   As suggested in [ticket 416](https://github.com/PowerDNS/pdns/issues/416), we now log the SOA serial number after committing an AXFRed zone to the backend. Code in [commit 2385](http://wiki.powerdns.com/projects/trac/changeset/2385).
-   Pick up location of sqlite3 libraries using pkg-config. Implemented using a variation of the patch found in the, now closed, [ticket 380](https://github.com/PowerDNS/pdns/issues/380). Code in [commit 2386](http://wiki.powerdns.com/projects/trac/changeset/2386).
-   Documented 'pdnssec --verbose' flag is now accepted. Code in [commit 2384](http://wiki.powerdns.com/projects/trac/changeset/2384), closing [ticket 404](https://github.com/PowerDNS/pdns/issues/404).
-   'pdnssec --help' now lists all supported signing algorithms. Suggested by Jose Arthur Benetasso Villanova.
-   PIPE backend example script with edns-subnet support was improved to actually use edns-subnet field. Plus update PIPE backend documentation. Code in [commit 2285](http://wiki.powerdns.com/projects/trac/changeset/2285), more documentation regarding MX and SRV in [commit 2313](http://wiki.powerdns.com/projects/trac/changeset/2313).
-   edns-subnet fields now also output in logfile when available ([commit 2321](http://wiki.powerdns.com/projects/trac/changeset/2321)).
-   When running with virtualized configuration files, we now allow dashes in the configuration name. Suggested by Marc Haber, code in [commit 2295](http://wiki.powerdns.com/projects/trac/changeset/2295). Further fixes by Brielle Bruns in [commit 2327](http://wiki.powerdns.com/projects/trac/changeset/2327).
-   Compilation fixes for GNU/Hurd in [commit 2307](http://wiki.powerdns.com/projects/trac/changeset/2307) via Matthijs Möhlmann.
-   Marc Haber improved our Debian packaging scripts for smoother upgrades. Code in [commit 2315](http://wiki.powerdns.com/projects/trac/changeset/2315).
-   When failing to bind to an IP address, report to which one it failed. [commit 2325](http://wiki.powerdns.com/projects/trac/changeset/2325).
-   Supermaster checks were performed synchronously, leading to the possibilities of slowdowns. Fixed in [commit 2402](http://wiki.powerdns.com/projects/trac/changeset/2402).

## Other changes
-   Removed the deprecated non-generic mysqlbackend, in [commit 2488](http://wiki.powerdns.com/projects/trac/changeset/2488), [commit 2514](http://wiki.powerdns.com/projects/trac/changeset/2514), [commit 2515](http://wiki.powerdns.com/projects/trac/changeset/2515).
-   Removed the deprecated 'pdnsbackend', in [commit 2490](http://wiki.powerdns.com/projects/trac/changeset/2490), [commit 2516](http://wiki.powerdns.com/projects/trac/changeset/2516).
-   Removed GRANT statements from the gpgsql schema, as we can't assume they will work for everyone. Change in [commit 2493](http://wiki.powerdns.com/projects/trac/changeset/2493).
Tickets closed but not associated with a commit
-   [ticket 125](https://github.com/PowerDNS/pdns/issues/125): "PowerDNS offers wild card info. when it is not queried for."
-   [ticket 219](https://github.com/PowerDNS/pdns/issues/219): "Accept NOTIFY from masters on non-standard port"
-   [ticket 247](https://github.com/PowerDNS/pdns/issues/247): "pdns caching weirdness with recursion-desired flag"
-   [ticket 253](https://github.com/PowerDNS/pdns/issues/253): "bind backend crashes on long comment line in included file"
-   [ticket 271](https://github.com/PowerDNS/pdns/issues/271): "PowerDNS Server responding with out-of-zone authority section in case there is a cname"
-   [ticket 304](https://github.com/PowerDNS/pdns/issues/304): "also-notify option for pdns, also gives also-notify for bindbackend."
-   [ticket 311](https://github.com/PowerDNS/pdns/issues/311): "PowerDNSSEC responding with SERVFAIL upon IN A query for a CNAME"
-   [ticket 325](https://github.com/PowerDNS/pdns/issues/325): "CNAME working strange!"
-   [ticket 376](https://github.com/PowerDNS/pdns/issues/376): "Unable to create long TXT records"
-   [ticket 412](https://github.com/PowerDNS/pdns/issues/412): "--without-lua doesn't disable lua"
-   [ticket 415](https://github.com/PowerDNS/pdns/issues/415): "Signing thread died during AXFR of signed domain"
-   [ticket 422](https://github.com/PowerDNS/pdns/issues/422): "ecdsa256 keys bug"

# Authoritative Server version 2.9.22.6
**Warning**: The 2.9.22.x series of releases is end-of-life and unsupported. It contains many issues and potential security problems. We urge you to upgrade to a recent version of PowerDNS!

The improvements to the master/slave engine in 2.9.22.5 contained one serious bug that can cause crashes on busy setups. 2.9.22.6 fixes this crash.

# Authoritative Server version 2.9.22.5
**Warning**: The 2.9.22.x series of releases is end-of-life and unsupported. It contains many issues and potential security problems. We urge you to upgrade to a recent version of PowerDNS!

2.9.22.5 is an interim release for those not yet ready to make the jump to 3.0, but do need a more recent version of the Authoritative Server. It also contains the patch from [PowerDNS Security Advisory 2012-01](security/powerdns-advisory-2012-01.md "PowerDNS Security Advisory 2012-01: PowerDNS Authoritative Server can be caused to generate a traffic loop").

-   Improved performance of master/slave engine, especially when hosting tens or hundreds of thousands of slave zones. Code in commits [1657](http://wiki.powerdns.com/projects/trac/changeset/1657), [1658](http://wiki.powerdns.com/projects/trac/changeset/1658), [1661](http://wiki.powerdns.com/projects/trac/changeset/1661) (which also brings multi-master support), [1662](http://wiki.powerdns.com/projects/trac/changeset/1662) (non-standard ports for masters), [1664](http://wiki.powerdns.com/projects/trac/changeset/1664), [1665](http://wiki.powerdns.com/projects/trac/changeset/1665), [1666](http://wiki.powerdns.com/projects/trac/changeset/1666), [1667](http://wiki.powerdns.com/projects/trac/changeset/1667), [1672](http://wiki.powerdns.com/projects/trac/changeset/1672), [1673](http://wiki.powerdns.com/projects/trac/changeset/1673), [2063](http://wiki.powerdns.com/projects/trac/changeset/2063)).
-   Compilation fixes for more modern compilers ([commit 1660](http://wiki.powerdns.com/projects/trac/changeset/1660), [commit 1694](http://wiki.powerdns.com/projects/trac/changeset/1694))
-   Don't crash on communication error with pdns\_control ([commit 2015](http://wiki.powerdns.com/projects/trac/changeset/2015)).
-   Packet cache fixes for UltraSPARC ([commit 1663](http://wiki.powerdns.com/projects/trac/changeset/1663))
-   Fix crashes in the BIND backend ([commit 1693](http://wiki.powerdns.com/projects/trac/changeset/1693), [commit 1692](http://wiki.powerdns.com/projects/trac/changeset/1692))

# PowerDNS Authoritative Server 3.0.1
**Warning**: The DNSSEC implementation of PowerDNS Authoritative Server 3.0 and 3.0.1 contains many issues regarding CNAMES, wildcards and (in)secure delegations. If you use any of these, and you use DNSSEC you MUST upgrade to 3.1 or beyond!

3.0.1 consists of 3.0, plus the patch from [PowerDNS Security Advisory 2012-01](security/powerdns-advisory-2012-01.md "PowerDNS Security Advisory 2012-01: PowerDNS Authoritative Server can be caused to generate a traffic loop")

# PowerDNS Authoritative Server 3.0
Released on the 22nd of July 2011
RC1 released on the 4th of April 2011
RC2 released on the 19th of April 2011
RC3 released on the 19th of July 2011

**Warning**: Version 3.0 of the PowerDNS Authoritative Server is a major upgrade if you are coming from 2.9.x. Please refer to the [Upgrade documentation](authoritative/upgrading.md) for important information on correct and stable operation, as well as notes on performance and memory use.

**Warning**: The DNSSEC implementation of PowerDNS Authoritative Server 3.0 and 3.0.1 contains many issues regarding CNAMES, wildcards and (in)secure delegations. If you use any of these, and you use DNSSEC you MUST upgrade to 3.1 or beyond!

Version 3.0 of the PowerDNS Authoritative Server brings a number of important features, as well as over two years of accumulated bug fixing.

The largest news in 3.0 is of course the advent of DNSSEC. Not only does PowerDNS now (finally) support DNSSEC, we think that our support of this important protocol is among the easiest to use available. In addition, all important algorithms are supported.

Complete detail can be found in [Serving authoritative DNSSEC data](authoritative/dnssec.md "Serving authoritative DNSSEC data"). The goal of 'PowerDNSSEC' is to allow existing PowerDNS installations to start serving DNSSEC with as little hassle as possible, while maintaining performance and achieving high levels of security.

Tutorials and examples of how to use DNSSEC in PowerDNS can be found linked from [http://powerdnssec.org](http://powerdnssec.org).

PowerDNS Authoritative Server 3.0 development has been made possible by the financial and moral support of

-   [AFNIC, the French registry](http://www.afnic.fr/)
-   [IPCom's RcodeZero Anycast DNS](http://www.ipcom.at/en/dns/rcodezero_anycast/), a subsidiary of NIC.AT, the Austrian registry
-   [SIDN, the Dutch registry](http://www.sidn.nl/)
-   .. (awaiting details) ..

This release has received exceptional levels of community support, and we'd like to thank the following people in addition to those mentioned explicitly below: Peter Koch (DENIC), Olaf Kolkman (NLNetLabs), Wouter Wijngaards (NLNetLabs), Marco Davids (SIDN), Markus Travaille (SIDN), Leen Besselink, Antoin Verschuren (SIDN), Olafur Guðmundsson (IETF), Dan Kaminsky (Recursion Ventures), Roy Arends (Nominet), Miek Gieben (SIDN), Stephane Bortzmeyer (AFNIC), Michael Braunoeder (nic.at), Peter van Dijk, Maik Zumstrull, Jose Arthur Benetasso Villanova (Locaweb), Stefan Schmidt, Roland van Rijswijk (Surfnet), Paul Bakker (Brainspark/Fox-IT), Mathew Hennessy, Johannes Kuehrer (Austrian World4You GmbH), Marc van de Geijn (bHosted.nl), Stefan Arentz and Martin van Hensbergen (Fox-IT), Christof Meerwald, Detlef Peeters, Jack Lloyd, Frank Altpeter, Fredrik Danerklint, Vasiliy G Tolstov, Brielle Bruns, Evan Hunt, Ralf van der Enden, Marc Laros, Serge Belyshev, Christian Hofstaedtler, Charlie Smurthwaite, Nikolaos Milas, ..

## Known issues as of RC3
-   Not all new features are fully documented yet

## Changes between RC3 and final
-   Slight tweak to the pipebackend to ease DNSSEC operations ([commit 2239](http://wiki.powerdns.com/projects/trac/changeset/2239), [commit 2247](http://wiki.powerdns.com/projects/trac/changeset/2247)). Also fix pipebackend support in pdnssec tool ([commit 2244](http://wiki.powerdns.com/projects/trac/changeset/2244)).
-   Upgrade the experimental native Lua backend to the latest version from Fredrik Danerklint ([commit 2240](http://wiki.powerdns.com/projects/trac/changeset/2240)) and include this backend in the .deb packages ([commit 2242](http://wiki.powerdns.com/projects/trac/changeset/2242))
-   Remove IPv6 dependency, it was only possible to run master/slave operations on a server with at least one IPv6 address. Some very old virtualized setups turned out to have no IPv6 at all. Fix in [commit 2246](http://wiki.powerdns.com/projects/trac/changeset/2246).

## Changes between RC2 and RC3
-   PowerDNS Authoritative Server could not be configured to use an IPv6 based resolving backend. Solved in [commit 2191](http://wiki.powerdns.com/projects/trac/changeset/2191).
-   LDAP backend reconfigured the timezone (TZ) setting of the daemon, leading to confusing logfile entries. Fixed by Christian Hofstaedtler in [commit 2913](http://wiki.powerdns.com/projects/trac/changeset/2913), closing [ticket 313](https://github.com/PowerDNS/pdns/issues/313).
-   Non-DNSSEC capable backends could crash on DNSSEC queries. Fixed in [commit 2194](http://wiki.powerdns.com/projects/trac/changeset/2194) and [commit 2196](http://wiki.powerdns.com/projects/trac/changeset/2196) (thanks to Charlie Smurthwaite) closing [ticket 360](https://github.com/PowerDNS/pdns/issues/360).
-   Errors looking up a UID or GID were reported confusingly ('Success'), fixed in [commit 2195](http://wiki.powerdns.com/projects/trac/changeset/2195), closing [ticket 359](https://github.com/PowerDNS/pdns/issues/359).
-   Fix compilation against older MySQL, client libraries ([commit 2198](http://wiki.powerdns.com/projects/trac/changeset/2198), [commit 2199](http://wiki.powerdns.com/projects/trac/changeset/2199), [commit 2204](http://wiki.powerdns.com/projects/trac/changeset/2204)), especially for older RHEL/CentOS. Also addresses the failure to look in lib64 directory for PostgreSQL.
-   Sqlite3 needs write access not just to its database file, but also to the directory it is in. If this wasn't the case, no useful error message was provided. Improvement in [commit 2202](http://wiki.powerdns.com/projects/trac/changeset/2202).
-   Update of MongoDB backend ([commit 2203](http://wiki.powerdns.com/projects/trac/changeset/2203), [commit 2212](http://wiki.powerdns.com/projects/trac/changeset/2212)).
-   'pdnssec hash-zone-record' emitted an inverted warning about narrow NSEC3 hashes. Spotted by Jan-Piet Mens, fix in [commit 2205](http://wiki.powerdns.com/projects/trac/changeset/2205).
-   PowerDNS can fill out default fields for SOA records, but neglected to do so if the SOA record was matched by an incoming ANY question. Spotted by Marc Laros & others. Fixes [ticket 357](https://github.com/PowerDNS/pdns/issues/357), code in [commit 2206](http://wiki.powerdns.com/projects/trac/changeset/2206).
-   PowerDNS would mistreat binary data in TXT records. Fix in [commit 2207](http://wiki.powerdns.com/projects/trac/changeset/2207). Again spotted by Jan-Piet Mens. Closes [ticket 356](https://github.com/PowerDNS/pdns/issues/356).
-   Add experimental Lua backend by our star contributor Fredrik Danerklint. [commit 2208](http://wiki.powerdns.com/projects/trac/changeset/2208).
-   Christoph Meerwald discovered our RRSIG freshness checking checked more than the intended RRSIG (on the SOA record). Fix in [commit 2209](http://wiki.powerdns.com/projects/trac/changeset/2209).
-   Christoph Meerwald discovered we got confused by TSIG signed EDNS-adorned queries, since we expected the EDNS OPT pseudorecord to be the very last record. Fix in [commit 2214](http://wiki.powerdns.com/projects/trac/changeset/2214).
-   Christoph Meerwald discovered that when using SOA outgoing editing we would sign and THEN edit. This was not productive. Fixed in [commit 2215](http://wiki.powerdns.com/projects/trac/changeset/2215).
-   Add missing-but-documented pdnssec command 'disable-dnssec'. Spotted by Craig Whitmore. Plus fixed misleading --help output. Code in [commit 2216](http://wiki.powerdns.com/projects/trac/changeset/2216).
-   By popular demand, a tweak which makes an overloaded database no longer restart PowerDNS but to drop queries until the database is available again. Code in [commit 2217](http://wiki.powerdns.com/projects/trac/changeset/2217), lightly tested. Enable by setting 'overload-queue-length=100' (for example).
-   By suggestion of Miek Gieben of SIDN, add SOA-EDIT mode 'EPOCH' which sets the SOA serial number to the 'UNIX time'. Implemented in [commit 2218](http://wiki.powerdns.com/projects/trac/changeset/2218).
-   Added some US export control & ECCN to documentation, needed because of DNSSEC content. Update in [commit 2219](http://wiki.powerdns.com/projects/trac/changeset/2219).
-   Fix up various spelling mistakes and badly formatted messages ([commit 2220](http://wiki.powerdns.com/projects/trac/changeset/2220) and [commit 2221](http://wiki.powerdns.com/projects/trac/changeset/2221)) by Maik Zumstrull and 'anonymous'.
-   After a lot of thought, we now handle CNAMEs to names outside our knowledge ('bailiwick') exactly as in BIND 9.8.0, even though our way was standards compliant too. It confused things. Update in [commit 2222](http://wiki.powerdns.com/projects/trac/changeset/2222) and [commit 2224](http://wiki.powerdns.com/projects/trac/changeset/2224).
-   Tweak sqlite3 library location detection for newer Ubuntu versions. Change in [commit 2223](http://wiki.powerdns.com/projects/trac/changeset/2223).
-   DNSSEC SQL schema improvements allowing for the use of constraints and foreign keys in [commit 2225](http://wiki.powerdns.com/projects/trac/changeset/2225), by Gerald Gruenberg, closing [ticket 371](https://github.com/PowerDNS/pdns/issues/371).
-   Add support for EDNS option 'edns-subnet', based on draft-vandergaast-edns-client-subnet ([commit 2226](http://wiki.powerdns.com/projects/trac/changeset/2226), [commit 2228](http://wiki.powerdns.com/projects/trac/changeset/2228), [commit 2229](http://wiki.powerdns.com/projects/trac/changeset/2229), [commit 2230](http://wiki.powerdns.com/projects/trac/changeset/2230), [commit 2231](http://wiki.powerdns.com/projects/trac/changeset/2231), [commit 2233](http://wiki.powerdns.com/projects/trac/changeset/2233)).
-   Zone2sql sent out the wrong 'COMMIT' statement in sqlite mode. In addition, in this mode, zone2sql would not emit statements to update the domains table unless the 'slave' setting was chosen. Code in [commit 2167](http://wiki.powerdns.com/projects/trac/changeset/2167).
-   We dropped the Authoritative Answer flag on an out-of-bailiwick CNAME referral, which was unnecessary. Code in [commit 2170](http://wiki.powerdns.com/projects/trac/changeset/2170).
-   Kees Monshouwer discovered that we failed to detect the location of PostgreSQL on RHEL/CentOS. Fix in [commit 2144](http://wiki.powerdns.com/projects/trac/changeset/2144). In addition, [commit 2162](http://wiki.powerdns.com/projects/trac/changeset/2162) eases detection of MySQL on RHEL/CentOS 64 bits systems.
-   Marc Laros re-reported an old bug in the internally used 'pdns' backend where details of the SOA record were not filled out correctly. Resolved in [commit 2145](http://wiki.powerdns.com/projects/trac/changeset/2145).
-   Jan-Piet Mens found that our TSIG signed SOA zone freshness check was signed incorrectly. Fixed in [commit 2147](http://wiki.powerdns.com/projects/trac/changeset/2147). Improved error messages that helped debug this issue in [commit 2148](http://wiki.powerdns.com/projects/trac/changeset/2148), [commit 2149](http://wiki.powerdns.com/projects/trac/changeset/2149).
-   Jan-Piet Mens helped debug an issue where some servers were "almost always" unable to transfer a TSIG signed zone correctly. Turns out that the TSIG signing code used an internal timestamp and not the remote timestamp. Because of good NTP synchronization this quite often was not a problem. Fix in [commit 2159](http://wiki.powerdns.com/projects/trac/changeset/2159).
-   Thor Spruyt of Telenet discovered that the PowerDNS code would try to emit DNS answers over TCP of over 65535 bytes long, which failed. We now truncate such answers properly. Code in [commit 2150](http://wiki.powerdns.com/projects/trac/changeset/2150).
-   The Slave engine now reuses an existing database connection, removing the need to create a new database connection every minute (and worse, log about it). Code in [commit 2153](http://wiki.powerdns.com/projects/trac/changeset/2153).
-   Fix a potential Year 2106 bug in the TSIG signing code. Because we care ([commit 2156](http://wiki.powerdns.com/projects/trac/changeset/2156)).
-   Added experimental support for the 'DANE' TLSA record which is used to authenticate SSL certificates via DNSSEC. [commit 2161](http://wiki.powerdns.com/projects/trac/changeset/2161).
-   Added experimental support for the MongoDB 'NoSQL' backend, contributed by Fredrik Danerklint in [commit 2162](http://wiki.powerdns.com/projects/trac/changeset/2162).

## Other major new features
-   TSIG for authorizing and authenticating AXFR requests & incoming zone transfers (Code in [2024](http://wiki.powerdns.com/projects/trac/changeset/2024), [2025](http://wiki.powerdns.com/projects/trac/changeset/2025), [2033](http://wiki.powerdns.com/projects/trac/changeset/2033), [2034](http://wiki.powerdns.com/projects/trac/changeset/2034)). This allows for retrieving TSIG protected content, as well as serving it.
-   Per zone also-notify.
-   MyDNS compatible backend, allowing for 'instantaneous' migration from this authoritative nameserver. Code in [commit 1418](http://wiki.powerdns.com/projects/trac/changeset/1418), contributed by Jonathan Oddy.
-   PowerDNS can now slave zones over IPv6 and notify IPv6 remotes of updates. Already. Code in [commit 2009](http://wiki.powerdns.com/projects/trac/changeset/2009) and beyond.
-   Lua based incoming zone editing, allowing masters or signing slaves to add information to the zone they will (re-)serve. Implemented in [commit 2065](http://wiki.powerdns.com/projects/trac/changeset/2065). To enable, use LUA-AXFR-SCRIPT zone metadata setting.
-   Native Oracle backend with full DNSSEC support. Contributed by Maik Zumstrull, then at the Steinbuch Centre for Computing at the Karlsruhe Institute of Technology.
-   "Also-notify" support, implemented by Aki Tuomi in [commit 1400](http://wiki.powerdns.com/projects/trac/changeset/1400). Support for Generic SQL backends and for the BIND backend. Further code in [commit 1360](http://wiki.powerdns.com/projects/trac/changeset/1360).
-   Support for binding to thousands of IP addresses, code in [commit 1443](http://wiki.powerdns.com/projects/trac/changeset/1443).
-   Generic MySQL backend now supports stored procedures. Implemented in [commit 2084](http://wiki.powerdns.com/projects/trac/changeset/2084), closing [ticket 231](https://github.com/PowerDNS/pdns/issues/231).
-   Generic ODBC backend compiles again, and is reported to work for some users that need it. Code contributed in [ticket 309](https://github.com/PowerDNS/pdns/issues/309), author unknown.
-   Massively parallel slaving infrastructure, able to check the freshness of thousands of remote zones per second, plus perform many incoming zone transfers simultaneously. Sponsored by Tyler Hall, code in [1449](http://wiki.powerdns.com/projects/trac/changeset/1449), [1500](http://wiki.powerdns.com/projects/trac/changeset/1500), [1859](http://wiki.powerdns.com/projects/trac/changeset/1859)
-   Core DNS logic replaced completely to deal with the brave new world of DNSSEC.

## Bugs fixed
-   sqlite2 and sqlite3 backends used MySQL-style escaping, leading to SQL errors in some cases. Discovered by Sten Spans. Fixed in [commit 1342](http://wiki.powerdns.com/projects/trac/changeset/1342).
-   Internal webserver no longer prints '1e2%'. Bug rediscovered by Jeff Sipek. Fixed in [commit 1342](http://wiki.powerdns.com/projects/trac/changeset/1342).
-   PowerDNS would refuse to serve domain names with spaces in them, or otherwise non-printable characters. Addressed in [commit 2081](http://wiki.powerdns.com/projects/trac/changeset/2081).
-   PowerDNS can now serve escaped labels, as described by RFC 4343. Data should be present in backends in that escaped form. Code in [commit 2089](http://wiki.powerdns.com/projects/trac/changeset/2089).
-   In some cases, we would include duplicate CNAMEs. In addition, we would hand out a full root-referral when not configured to in some cases (ticket [223](https://github.com/PowerDNS/pdns/issues/223)). Discovered by Andreas Jakum, fixed in [commit 1344](http://wiki.powerdns.com/projects/trac/changeset/1344).
-   Shane Kerr discovered we would corrupt DNS transaction IDs from the packet cache on big endian systems. Fix in [commit 1346](http://wiki.powerdns.com/projects/trac/changeset/1346), closing [ticket 222](https://github.com/PowerDNS/pdns/issues/222).
-   PowerDNS did not use RFC 1982 serial arithmetic, leading to a SOA serial number of 1 to be regarded as older than 4400000000, when in fact it is 'newer'. Issue (re-)discovered by Jan-Piet Mens.
-   BIND backend got confused of a zone's file name changed after a configuration reload. Fix in [commit 1347](http://wiki.powerdns.com/projects/trac/changeset/1347), closing [ticket 228](https://github.com/PowerDNS/pdns/issues/228).
-   When restarted by the Guardian, PowerDNS will perform a full multi-threaded cache cleanup, which took a long time and could crash. Fix in [commit 1364](http://wiki.powerdns.com/projects/trac/changeset/1364).
-   Under artificial circumstances, PowerDNS would never clean its packet cache. Found by Marcus Goller, fix in [commit 1399](http://wiki.powerdns.com/projects/trac/changeset/1399) and [commit 1408](http://wiki.powerdns.com/projects/trac/changeset/1408). This update also retunes the cleanup frequency.
-   Packetcache would cache things it should not have been caching. Fixes in commits [1407](http://wiki.powerdns.com/projects/trac/changeset/1407), [1488](http://wiki.powerdns.com/projects/trac/changeset/1488), [1869](http://wiki.powerdns.com/projects/trac/changeset/1869), [1880](http://wiki.powerdns.com/projects/trac/changeset/1880)
-   When processing incoming notifications, the BIND backend was case-sensitive, and would disregard notifications in the wrong case. Discovered by 'Dolphin', fix in [commit 1420](http://wiki.powerdns.com/projects/trac/changeset/1420).
-   The init.d script did not mention the 'reload' command. Code in [commit 1463](http://wiki.powerdns.com/projects/trac/changeset/1463), closes [ticket 233](https://github.com/PowerDNS/pdns/issues/233).
-   Generic SQL Backends would sometimes emit obscure error messages. Fix in [commit 2049](http://wiki.powerdns.com/projects/trac/changeset/2049).
-   PowerDNS would be confused by embedded NULs in domain names, and would also mess up the escaping of some characters. Fix in [commit 1468](http://wiki.powerdns.com/projects/trac/changeset/1468), [commit 1469](http://wiki.powerdns.com/projects/trac/changeset/1469), [commit 1478](http://wiki.powerdns.com/projects/trac/changeset/1478), [commit 1480](http://wiki.powerdns.com/projects/trac/changeset/1480),
-   SOA queries for the name of a delegation point were not referred. Fix in [commit 1466](http://wiki.powerdns.com/projects/trac/changeset/1466), closing [ticket 224](https://github.com/PowerDNS/pdns/issues/224). In addition, queries for AAAA for a CNAMEd record pointing to a name with no AAAA would deliver a direct SOA, without the CNAME in between. Fix in [commit 1542](http://wiki.powerdns.com/projects/trac/changeset/1542), [commit 1607](http://wiki.powerdns.com/projects/trac/changeset/1607). Also, wildcard CNAMEs pointing to a record without the type requested suffered from the same issue, fix in [commit 1543](http://wiki.powerdns.com/projects/trac/changeset/1543).
-   On processing an incoming AXFR, once an MX or SRV record had been seen, all future fields got a 'priority' entry as well. This had no operational impact, but looked messy. Fixed in [commit 1437](http://wiki.powerdns.com/projects/trac/changeset/1437).
-   Aki Tuomi discovered that the BIND zone file parser would misrepresent 'something IN MX 15 @'. Fix in [commit 1621](http://wiki.powerdns.com/projects/trac/changeset/1621).
-   Marco Davids discovered the BIND zone file parser would trip over really long lines. Fix in [commit 1624](http://wiki.powerdns.com/projects/trac/changeset/1624), [commit 1625](http://wiki.powerdns.com/projects/trac/changeset/1625).
-   Thomas Mieslinger discovered that our webserver would only be started after dropping privileges, which could cause problems. Fix in [commit 1629](http://wiki.powerdns.com/projects/trac/changeset/1629).
-   Zone2sql did quite often not do exactly what was required, which users fixed by editing the SQL output. Revamped in [commit 2032](http://wiki.powerdns.com/projects/trac/changeset/2032).
-   An Ubuntu user discovered in Launchpad bug 600479 that restarting database threads cost a lot of memory. Normally this is rare, except in case of problems. Addressed in [commit 1676](http://wiki.powerdns.com/projects/trac/changeset/1676).
-   BIND backend could crash under (very) high load with very large numbers of zones (hundreds of thousands). Fixed in [commit 1690](http://wiki.powerdns.com/projects/trac/changeset/1690).
-   Miek Gieben and Marco Davids spotted that PowerDNS would answer the version.bind query in the IN class too. Bug reported via twitter! Fix in [commit 1709](http://wiki.powerdns.com/projects/trac/changeset/1709).
-   Marcus Lauer and the OpenDNSSEC project discovered that outgoing notifications did not carry the 'aa' flag. Fixed in [commit 1746](http://wiki.powerdns.com/projects/trac/changeset/1746).
-   Debugging PowerDNS, or backgrounding it, could cause crashes. Fixed by Anders Kaseorg in [commit 1747](http://wiki.powerdns.com/projects/trac/changeset/1747).
-   Fixed a bug that could cause crashes on launching thousands of backend connections. Never observed to occur, but who knows. Fix in [commit 1792](http://wiki.powerdns.com/projects/trac/changeset/1792).
-   Under some circumstances, large answers could be truncated in mid-record. While technically legal, this upset a number of resolver implementations (including the PowerDNS Recursor!). Fixed in [commit 1830](http://wiki.powerdns.com/projects/trac/changeset/1830), re-closes [ticket 200](https://github.com/PowerDNS/pdns/issues/200).
-   Jan Piet Mens and Florian Weimer discovered we had problems dealing with escaped labels and escaped TXT fields. Fixed in [commit 2000](http://wiki.powerdns.com/projects/trac/changeset/2000).
-   After 2.2 billion queries, statistics would wrap oddly. Fix in [commit 2019](http://wiki.powerdns.com/projects/trac/changeset/2019), closing [ticket 327](https://github.com/PowerDNS/pdns/issues/327).

## Improvements
-   Long TXT records are now split into 255-byte components automatically. Implemented in [commit 1340](http://wiki.powerdns.com/projects/trac/changeset/1340), reported by Darren Gamble in [ticket 188](https://github.com/PowerDNS/pdns/issues/188).
-   When receiving large numbers of notifications, PowerDNS would check these synchronously, leading to a slowdown for other services. Fixed in [commit 2058](http://wiki.powerdns.com/projects/trac/changeset/2058), problem diagnosed by Richard Poole of Heart Internet.
-   Fixed compilation on newer compilers and newer versions of Boost. Changes in [1345](http://wiki.powerdns.com/projects/trac/changeset/1345) (closes [ticket 227](https://github.com/PowerDNS/pdns/issues/227)), [1391](http://wiki.powerdns.com/projects/trac/changeset/1391), [1394](http://wiki.powerdns.com/projects/trac/changeset/1394), [1425](http://wiki.powerdns.com/projects/trac/changeset/1425), [1427](http://wiki.powerdns.com/projects/trac/changeset/1427), [1428](http://wiki.powerdns.com/projects/trac/changeset/1428), [1429](http://wiki.powerdns.com/projects/trac/changeset/1429), [1440](http://wiki.powerdns.com/projects/trac/changeset/1440), [1653](http://wiki.powerdns.com/projects/trac/changeset/1653), thanks to Ruben Kerkhof and others.
-   Moved Generic PostgreSQL backend over to the newer E'' style escapes. [commit 2094](http://wiki.powerdns.com/projects/trac/changeset/2094).
-   Compilation fixes for Mac OS X 10.5.7 in [commit 1389](http://wiki.powerdns.com/projects/trac/changeset/1389), thanks to Tobias Markmann.
-   We can now bind to scoped IPv6 addresses, lack spotted by Darren Gamble. Part of the fix is in [commit 2018](http://wiki.powerdns.com/projects/trac/changeset/2018).
-   Built-in query cache can now also cache queries which lead to multiple answers. Code in [commit 2069](http://wiki.powerdns.com/projects/trac/changeset/2069).
-   Prodded on by Jan Piet Mens, we now support 'unknown types' (which look like TYPE65534).
-   Add 'slave-renotify' to retransmit notifies for slaved zones, which is helpful when acting as a 'signing slave' for a hidden master. Code in [commit 1950](http://wiki.powerdns.com/projects/trac/changeset/1950).
-   No longer let zone2sql and zone2ldap import BIND 'hint' zones. [commit 1998](http://wiki.powerdns.com/projects/trac/changeset/1998).
-   Allow for timestamps to explicitly be specified in (s)econds. Code in [commit 1398](http://wiki.powerdns.com/projects/trac/changeset/1398), closing [ticket 250](https://github.com/PowerDNS/pdns/issues/250).
-   Zones with URL and MBOXFW records can be transferred over AXFR, code in [commit 1464](http://wiki.powerdns.com/projects/trac/changeset/1464).
-   Maik Zumstrull cleaned up the BIND Backend makefile, plus taught our init.d script to read /etc/default/pdns. Code in [commit 1601](http://wiki.powerdns.com/projects/trac/changeset/1601), [commit 1602](http://wiki.powerdns.com/projects/trac/changeset/1602).
-   Generic SQL backends now support multiple masters in the domains table. Code in [commit 1857](http://wiki.powerdns.com/projects/trac/changeset/1857). Additionally, masters can also have :port numbers. Code in [commit 1858](http://wiki.powerdns.com/projects/trac/changeset/1858).

# Recursor version 3.3.1
**Warning**:Unreleased

Version 3.3.1 contains a small number of important fixes, adds some memory usage statistics, but no new features.

-   Discovered by John J and Robin J, the PowerDNS Recursor did not process packets that were truncated in mid-record, and also did not act on the 'truncated' (TC) flag in that case. This broke a very small number of domains, most of them served by very old versions of the PowerDNS Authoritative Server. Fix in [commit 1740](http://wiki.powerdns.com/projects/trac/changeset/1740).
-   PowerDNS emitted a harmless, but irritating, error message on receiving certain very short packets. Discovered by Winfried A and John J, fix in [commit 1729](http://wiki.powerdns.com/projects/trac/changeset/1729).
-   PowerDNS could crash on startup if configured to provide service on malformed IPv6 addresses on FreeBSD, or in case when the FreeBSD kernel was compiled without any form of IPv6 support. Debugged by Bryan Seitz, fix in [commit 1727](http://wiki.powerdns.com/projects/trac/changeset/1727).
-   Add max-mthread-stack metric to debug rare crashes. Could be used to save memory on constrained systems. Implemented in [commit 1745](http://wiki.powerdns.com/projects/trac/changeset/1745).
-   Add cache-bytes and packetcache-bytes metrics to measure our 'pre-malloc' memory utilization. Implemented in [commit 1750](http://wiki.powerdns.com/projects/trac/changeset/1750).

# Recursor version 3.3
Released on the 22nd of September 2010.

**Warning**: Version 3.3 fixes a number of small but persistent issues, rounds off our IPv6 %link-level support and adds an important feature for many users of the Lua scripts.

In addition, scalability on Solaris 10 is improved.

## Bug fixes
-   'dist-recursor' script was not compatible with pure POSIX /bin/sh, discovered by Simon Kirby. Fix in [commit 1545](http://wiki.powerdns.com/projects/trac/changeset/1545).
-   Simon Bedford, Brad Dameron and Laurient Papier discovered relatively high TCP/IP loads could cause TCP/IP service to shut down over time. Addressed in commits [1546](http://wiki.powerdns.com/projects/trac/changeset/1546), [1640](http://wiki.powerdns.com/projects/trac/changeset/1640), [1652](http://wiki.powerdns.com/projects/trac/changeset/1652), [1685](http://wiki.powerdns.com/projects/trac/changeset/1685), [1698](http://wiki.powerdns.com/projects/trac/changeset/1698). Additional information provided by Zwane Mwaikambo, Nicholas Miell and Jeff Roberson. Testing by Christian Hofstaedtler and Michael Renner.
-   The PowerDNS Recursor could not read the 'root zone' (this is something else than the root hints) because of an unquoted TXT record. This has now been addressed, allowing operators to hardcode the root zone. This can improve security if the root zone used is kept up to date. Change in [commit 1547](http://wiki.powerdns.com/projects/trac/changeset/1547).
-   A return of an old bug, when a domain gets new nameservers, but the old nameservers continue to contain a copy of the domain, PowerDNS could get 'stuck' with the old servers. Fixed in [commit 1548](http://wiki.powerdns.com/projects/trac/changeset/1548).
-   Discovered & reported by Alexander Gall of SWITCH, the Recursor used to try to resolve 'AXFR' records over UDP. Fix in [commit 1619](http://wiki.powerdns.com/projects/trac/changeset/1619).
-   The Recursor embedded authoritative server messed up parsing a record like '@ IN MX 15 @'. Spotted by Aki Tuomi, fix in [commit 1621](http://wiki.powerdns.com/projects/trac/changeset/1621).
-   The Recursor embedded authoritative server messed up parsing really really long lines. Spotted by Marco Davids, fix in [commit 1624](http://wiki.powerdns.com/projects/trac/changeset/1624), [commit 1625](http://wiki.powerdns.com/projects/trac/changeset/1625).
-   Packet cache was not DNS class correct. Spotted by "Robin", fix in [commit 1688](http://wiki.powerdns.com/projects/trac/changeset/1688).
-   The packet cache would cache some NXDOMAINs for too long. Solving this bug exposed an underlying oddity where the initial NXDOMAIN response had an overly long (untruncated) TTL, whereas all the next ones would be ok. Solved in [commit 1679](http://wiki.powerdns.com/projects/trac/changeset/1679), closing [ticket 281](https://github.com/PowerDNS/pdns/issues/281). Especially important for RBL operators. Fixed after some nagging by Alex Broens (thanks).

## Improvements
-   The priming of the root now uses more IPv6 addresses. Change in [commit 1550](http://wiki.powerdns.com/projects/trac/changeset/1550), closes [ticket 287](https://github.com/PowerDNS/pdns/issues/287). Also, the IPv6 address of I.ROOT-SERVERS.NET was added in [commit 1650](http://wiki.powerdns.com/projects/trac/changeset/1650).
-   The `rec_control dump-cache` command now also dumps the 'negative query' cache. Code in [commit 1713](http://wiki.powerdns.com/projects/trac/changeset/1713).
-   PowerDNS Recursor can now bind to fe80 IPv6 space with '%eth0' link selection. Suggested by Darren Gamble, implemented with help from Niels Bakker. Change in [commit 1620](http://wiki.powerdns.com/projects/trac/changeset/1620).
-   Solaris on x86 has a long standing bug in port\_getn(), which we now work around. Spotted by 'Dirk' and 'AS'. Solution suggested by the Apache runtime library, update in [commit 1622](http://wiki.powerdns.com/projects/trac/changeset/1622).
-   New runtime statistic: 'tcp-clients' which lists the number of currently active TCP/IP clients. Code in [commit 1623](http://wiki.powerdns.com/projects/trac/changeset/1623).
-   Deal better with UltraDNS style CNAME redirects containing SOA records. Spotted by Andy Fletcher from UKDedicated in [ticket 303](https://github.com/PowerDNS/pdns/issues/303), fix in [commit 1628](http://wiki.powerdns.com/projects/trac/changeset/1628).
-   The packet cache, which has 'ready to use' packets containing answers, now artificially ages the ready to use packets. Code in [commit 1630](http://wiki.powerdns.com/projects/trac/changeset/1630).
-   Lua scripts can now indicate that certain queries will have 'variable' answers, which means that the packet cache will not touch these answers. This is great for overriding some domains for some users, but not all of them. Use setvariable() in Lua to indicate such domains. Code in [commit 1636](http://wiki.powerdns.com/projects/trac/changeset/1636).
-   Add query statistic called 'dont-outqueries', plus add IPv6 address :: and IPv4 address 0.0.0.0 to the default "dont-query" set, preventing the Recursor from talking to itself. Code in [commit 1637](http://wiki.powerdns.com/projects/trac/changeset/1637).
-   Work around a gcc 4.1 bug, still in wide use on common platforms. Code in [commit 1653](http://wiki.powerdns.com/projects/trac/changeset/1653).
-   Add 'ARCHFLAGS' to PowerDNS Recursor Makefile, easing 64 bit compilation on mainly 32 bit platforms (and vice versa).
-   Under rare circumstances, querying the Recursor for statistics under very high load could lead to a crash (although this has never been observed). Bad code removed & good code unified in [commit 1675](http://wiki.powerdns.com/projects/trac/changeset/1675).
-   Spotted by Jeff Sipek, the rec\_control manpage did not list the new get-all command. [commit 1677](http://wiki.powerdns.com/projects/trac/changeset/1677).
-   On some platforms, it may be better to have PowerDNS itself distribute queries over threads (instead of leaving it up to the kernel). This experimental feature can be enabled with the 'pdns-distributes-queries' setting. Code in [commit 1678](http://wiki.powerdns.com/projects/trac/changeset/1678) and beyond. Speeds up Solaris measurably.
-   Cache cleaning code was cleaned up, unified and expanded to cover the 'negative cache', which used to be cleaned rather bluntly. Code in [commit 1702](http://wiki.powerdns.com/projects/trac/changeset/1702), further tweaks in [commit 1712](http://wiki.powerdns.com/projects/trac/changeset/1712), spotted by Darren Gamble, Imre Gergely and Christian Kovacic.

## Changes between RC1, RC2 and RC3.
-   RC2: Fixed linking on RHEL5/CentOS5, which both ship with a gcc compiler that claims to support atomic operations, but doesn't. Code in [commit 1714](http://wiki.powerdns.com/projects/trac/changeset/1714). Spotted by 'Bas' and Imre Gergely.
-   RC2: Negative query cache was configured to grow too large, and was not cleaned efficiently. Code in [commit 1712](http://wiki.powerdns.com/projects/trac/changeset/1712), spotted by Imre Gergely.
-   RC3: Root failed to be renewed automatically, relied on fallback to make this happen. Code in [commit 1716](http://wiki.powerdns.com/projects/trac/changeset/1716), spotted by Detlef Peeters.

# Recursor version 3.2
Released on the 7th of March 2010.

**Warning**: Lua scripts from version 3.1.7.* are fully compatible with version 3.2. However, scripts written for development snapshot releases, are NOT. Please see [Scripting](recursor/scripting.md "Scripting") for details!

The 3.2 release is the first major release of the PowerDNS Recursor in a long time. Partly this is because 3.1.7.* functioned very well, and delivered satisfying performance, partly this is because in order to really move forward, some heavy lifting had to be done.

As always, we are grateful for the large PowerDNS community that is actively involved in improving the quality of our software, be it by submitting patches, by testing development versions of our software or helping debug interesting issues. We specifically want to thank Stefan Schmidt and Florian Weimer, who both over the years have helped tremendously in keeping PowerDNS fast, stable and secure.

This version of the PowerDNS Recursor contains a rather novel form of lock-free multithreading, a situation that comes close to the old '--fork' trick, but allows the Recursor to fully utilize multiple CPUs, while delivering unified statistics and operational control.

In effect, this delivers the best of both worlds: near linear scaling, with almost no administrative overhead.

Compared to 'regular multithreading', whereby threads cooperate more closely, more memory is used, since each thread maintains its own DNS cache. However, given the economics, and the relatively limited total amount of memory needed for high performance, this price is well worth it.

In practical numbers, over 40,000 queries/second sustained performance has now been measured by a third party, with a 100.0% packet response rate. This means that the needs of around 400,000 residential connections can now be met by a single commodity server.

In addition to the above, the PowerDNS Recursor is now providing resolver service for many more Internet users than ever before. This has brought with it 24/7 Service Level Agreements, and 24/7 operational monitoring by networking personnel at some of the largest telecommunications companies in the world.

In order to facilitate such operation, more statistics are now provided that allow the visual verification of proper PowerDNS Recursor operation. As an example of this there are now graphs that plot how many queries were dropped by the operating system because of a CPU overload, plus statistics that can be monitored to determine if the PowerDNS deployment is under a spoofing attack.
All in all, this is a large and important PowerDNS Release, paving the way for further innovation.

**Note**: This release removes support for the 'fork' multi-processor option. In addition, the default is now to spawn two threads. This has been done in such a way that total memory usage will remain identical, so each thread will use half of the allocated maximum number of cache entries.

## Changes between RC2 and -release
-   'Make install' when an existing configuration file contained a 'fork' statement has been fixed. Spotted by Darren Gamble, code in [commit 1534](http://wiki.powerdns.com/projects/trac/changeset/1534).
-   Reloading a non-existent allow-from-file caused the control thread to stop working. Spotted by Imre Gergely, code in [commit 1532](http://wiki.powerdns.com/projects/trac/changeset/1532).
-   Parser got confused by reading en empty line in auth-forward-zones. Spotted by Imre Gergely, code in [commit 1533](http://wiki.powerdns.com/projects/trac/changeset/1533).
-   David Gavarret discovered undocumented and not-working settings to set the owner, group and access modes of the control socket. Code by Aki Tuomi and documentation in [commit 1535](http://wiki.powerdns.com/projects/trac/changeset/1535). Fixup in [commit 1536](http://wiki.powerdns.com/projects/trac/changeset/1536) for FreeBSD as found by Ralf van der Enden.
-   Tiny improvement possibly solving an issue on Solaris 10's completion port event multiplexer ([commit 1537](http://wiki.powerdns.com/projects/trac/changeset/1537)).

## Changes between RC1 and RC2
-   Compilation on Solaris 10 has been fixed (various patchlevels had different issues), code in [commit 1522](http://wiki.powerdns.com/projects/trac/changeset/1522).
-   Compatibility with CentOS4/RHEL4 has been restored, the gcc and glibc versions shipped with this distribution contain a Thread Local Storage bug which we now work around. Thanks to Darren Gamble and Imre Gergely for debugging this issue, code in [commit 1527](http://wiki.powerdns.com/projects/trac/changeset/1527).
-   A failed setuid operation, because of misconfiguration, would result in a crash instead of an error message. Fixed in [commit 1523](http://wiki.powerdns.com/projects/trac/changeset/1523).
-   Imre Gergely discovered that PowerDNS was doing spurious root repriming when invalidating nssets. Fixed in [commit 1531](http://wiki.powerdns.com/projects/trac/changeset/1531).
-   Imre Gergely discovered our rrd graphs had not been changed for the new multithreaded world, and did not allow scaling beyond 200% cpu use. In addition, CPU usage graphs did not add up correctly. Implemented in [commit 1524](http://wiki.powerdns.com/projects/trac/changeset/1524).
-   Andreas Jakum discovered the description of 'max-packetcache-entries' and 'forward-zones-recurse' was wrong in the output of '--help' and '--config'. In addition, some stray backup files made it into the RC1 release. Addressed in [commit 1529](http://wiki.powerdns.com/projects/trac/changeset/1529).
Full release notes follow, including some overlap with the incremental release notes above. Improvements
-   Multithreading, allowing near linear scaling to multiple CPUs or cores. Configured using 'threads=' (many commits). This also deprecates the '--fork' option.
-   Added ability to read a configuration item of a running PowerDNS Recursor using 'rec\_control get-parameter' ([commit 1243](http://wiki.powerdns.com/projects/trac/changeset/1243)), suggested by Wouter de Jong.
-   Added ability to read all statistics in one go of a running PowerDNS Recursor using 'rec\_control get-all' ([commit 1496](http://wiki.powerdns.com/projects/trac/changeset/1496)), suggested by Michael Renner.
-   Speedups in packet generation (Commits [1258](http://wiki.powerdns.com/projects/trac/changeset/1258), [1259](http://wiki.powerdns.com/projects/trac/changeset/1259), [1262](http://wiki.powerdns.com/projects/trac/changeset/1262))
-   TCP deferred accept() filter is turned on again for slight DoS protection. Code in [commit 1414](http://wiki.powerdns.com/projects/trac/changeset/1414).
-   PowerDNS Recursor can now do TCP/IP queries to remote IPv6 addresses ([commit 1412](http://wiki.powerdns.com/projects/trac/changeset/1412)).
-   Solaris 9 '/dev/poll' support added, Solaris 8 now deprecated. Changes in [commit 1421](http://wiki.powerdns.com/projects/trac/changeset/1421), [commit 1422](http://wiki.powerdns.com/projects/trac/changeset/1422), [commit 1424](http://wiki.powerdns.com/projects/trac/changeset/1424), [commit 1413](http://wiki.powerdns.com/projects/trac/changeset/1413).
-   Lua functions can now also see the address \_to\_ which a question was sent, using getlocaladdress(). Implemented in [commit 1309](http://wiki.powerdns.com/projects/trac/changeset/1309) and [commit 1315](http://wiki.powerdns.com/projects/trac/changeset/1315).
-   Maximum cache sizes now default to a sensible value. Suggested by Roel van der Made, implemented in [commit 1354](http://wiki.powerdns.com/projects/trac/changeset/1354).
-   Domains can now be forwarded to IPv6 addresses too, using either ::1 syntax or [::1]:25. Thanks to Wijnand Modderman for discovering this issue, fixed in [commit 1349](http://wiki.powerdns.com/projects/trac/changeset/1349).
-   Lua scripts can now load libraries at runtime, for example to calculate md5 hashes. Code by Winfried Angele in [commit 1405](http://wiki.powerdns.com/projects/trac/changeset/1405).
-   Periodic statistics output now includes average queries per second, as well as packet cache numbers ([commit 1493](http://wiki.powerdns.com/projects/trac/changeset/1493)).
-   New metrics are available for graphing, plus added to the default graphs ([commit 1495](http://wiki.powerdns.com/projects/trac/changeset/1495), [commit 1498](http://wiki.powerdns.com/projects/trac/changeset/1498), [commit 1503](http://wiki.powerdns.com/projects/trac/changeset/1503))
-   Fix errors/crashes on more recent versions of Solaris 10, where the ports functions could return ENOENT under some circumstances. Reported and debugged by Jan Gyselinck, fixed in [commit 1372](http://wiki.powerdns.com/projects/trac/changeset/1372).

## New features
-   Add pdnslog() function for Lua scripts, so errors or other messages can be logged properly.
-   New settings to set the owner, group and access modes of the control socket (socket-owner, socket-group, socket-mode). Code by Aki Tuomi and documentation in [commit 1535](http://wiki.powerdns.com/projects/trac/changeset/1535). Fixup in [commit 1536](http://wiki.powerdns.com/projects/trac/changeset/1536) for FreeBSD as found by Ralf van der Enden.
-   rec\_control now accepts a --timeout parameter, which can be useful when reloading huge Lua scripts. Implemented in [commit 1366](http://wiki.powerdns.com/projects/trac/changeset/1366).
-   Domains can now be forwarded with the 'recursion-desired' bit on or off, using either **forward-zones-recurse** or by prefixing the name of a zone with a '+' in **forward-zones-file**. Feature suggested by Darren Gamble, implemented in [commit 1451](http://wiki.powerdns.com/projects/trac/changeset/1451).
-   Access control lists can now be reloaded at runtime (implemented in [commit 1457](http://wiki.powerdns.com/projects/trac/changeset/1457)).
-   PowerDNS Recursor can now use a pool of query-local-addresses to further increase resilience against spoofing. Suggested by Ad Spelt, implemented in [commit 1426](http://wiki.powerdns.com/projects/trac/changeset/1426).
-   PowerDNS Recursor now also has a packet cache, greatly speeding up operations. Implemented in [commit 1426](http://wiki.powerdns.com/projects/trac/changeset/1426), [commit 1433](http://wiki.powerdns.com/projects/trac/changeset/1433) and further.
-   Cache can be limited in how long it maximally stores records, for BIND compatibility (TTL limiting), by setting **max-cache-ttl**.Idea by Winfried Angele, implemented in [commit 1438](http://wiki.powerdns.com/projects/trac/changeset/1438).
-   Cache cleaning turned out to be scanning more of the cache than necessary for cache maintenance. In addition, far more frequent but smaller cache cleanups improve responsiveness. Thanks to Winfried Angele for discovering this issue. (commits [1501](http://wiki.powerdns.com/projects/trac/changeset/1501), [1507](http://wiki.powerdns.com/projects/trac/changeset/1507))
-   Performance graphs enhanced with separate CPU load and cache effectiveness plots, plus display of various overload situations (commits [1503](http://wiki.powerdns.com/projects/trac/changeset/1503))

## Compiler/Operating system/Library updates
-   PowerDNS Recursor can now compile against newer versions of Boost (verified up to and including 1.42.0). Reported & fixed by Darix in [commit 1274](http://wiki.powerdns.com/projects/trac/changeset/1274). Further fixes in [commit 1275](http://wiki.powerdns.com/projects/trac/changeset/1275), [commit 1276](http://wiki.powerdns.com/projects/trac/changeset/1276), [commit 1277](http://wiki.powerdns.com/projects/trac/changeset/1277), [commit 1283](http://wiki.powerdns.com/projects/trac/changeset/1283).
-   Fix compatibility with newer versions of GCC (closes ticket [ticket 227](https://github.com/PowerDNS/pdns/issues/227), spotted by Ruben Kerkhof, code in [commit 1345](http://wiki.powerdns.com/projects/trac/changeset/1345), more fixes in commit [1394](http://wiki.powerdns.com/projects/trac/changeset/1394), [1416](http://wiki.powerdns.com/projects/trac/changeset/1416), [1440](http://wiki.powerdns.com/projects/trac/changeset/1440)).
-   Rrdtool update graph is now compatible with FreeBSD out of the box. Thanks to Bryan Seitz ([commit 1517](http://wiki.powerdns.com/projects/trac/changeset/1517)).
-   Fix up Makefile for older versions of Make ([commit 1229](http://wiki.powerdns.com/projects/trac/changeset/1229)).
-   Solaris compilation improvements (out of the box, no handwork needed).
-   Solaris 9 MTasker compilation fixes, as suggested by John Levon. Changes in [commit 1431](http://wiki.powerdns.com/projects/trac/changeset/1431).

## Bug fixes
-   Under rare circumstances, the recursor could crash on 64 bit Linux systems running glibc 2.7, as found in Debian Lenny. These circumstances became a lot less rare for the 3.2 release. Discovered by Andreas Jakum and debugged by \#powerdns, fix in [commit 1519](http://wiki.powerdns.com/projects/trac/changeset/1519).
-   Imre Gergely discovered that PowerDNS was doing spurious root repriming when invalidating nssets. Fixed in [commit 1531](http://wiki.powerdns.com/projects/trac/changeset/1531).
-   Configuration parser is now resistant against trailing tabs and other whitespace ([commit 1242](http://wiki.powerdns.com/projects/trac/changeset/1242))
-   Fix typo in a Lua error message. Close [ticket 210](https://github.com/PowerDNS/pdns/issues/210), as reported by Stefan Schmidt ([commit 1319](http://wiki.powerdns.com/projects/trac/changeset/1319)).
-   Profiled-build instructions were broken, discovered & fixes suggested by Stefan Schmidt. [ticket 239](https://github.com/PowerDNS/pdns/issues/239), fix in [commit 1462](http://wiki.powerdns.com/projects/trac/changeset/1462).
-   Fix up duplicate SOA from a remote authoritative server from showing up in our output ([commit 1475](http://wiki.powerdns.com/projects/trac/changeset/1475)).
-   All security fixes from 3.1.7.2 are included.
-   Under highly exceptional circumstances on FreeBSD the PowerDNS Recursor could crash because of a TCP/IP error. Reported and fixed by Andrei Poelov in [ticket 192](https://github.com/PowerDNS/pdns/issues/192), fixed in [commit 1280](http://wiki.powerdns.com/projects/trac/changeset/1280).
-   PowerDNS Recursor can be a root-server again. Error spotted by the ever vigilant Darren Gamble (ticket [229](https://github.com/PowerDNS/pdns/issues/229)), fix in [commit 1458](http://wiki.powerdns.com/projects/trac/changeset/1458).
-   Rare TCP/IP errors no longer lead to PowerDNS Recursor logging errors or becoming confused. Debugged by Josh Berry of Plusnet PLC. Code in [commit 1457](http://wiki.powerdns.com/projects/trac/changeset/1457).
-   Do not hammer parent servers in case child zones are misconfigured, requery at most once every 10 seconds. Reported & investigated by Stefan Schmidt and Andreas Jakum, fixed in [commit 1265](http://wiki.powerdns.com/projects/trac/changeset/1265).
-   Properly process answers from remote authoritative servers that send error answers without including the original question ([commit 1329](http://wiki.powerdns.com/projects/trac/changeset/1329), [commit 1327](http://wiki.powerdns.com/projects/trac/changeset/1327)).
-   No longer spontaneously turn on 'export-etc-hosts' after reloading zones. Discovered by Paul Cairney, reported in [ticket 225](https://github.com/PowerDNS/pdns/issues/225), addressed in [commit 1348](http://wiki.powerdns.com/projects/trac/changeset/1348).
-   Very abrupt server failure of large numbers of high-volume authoritative servers could trigger an out of memory situation. Addressed in [commit 1505](http://wiki.powerdns.com/projects/trac/changeset/1505).
-   Make timeouts for queries to remote authoritative servers configurable with millisecond granularity. In addition, the old code turned out to consider the timeout expired when the integral number of seconds since 1970 increased by 1 - which *on average* is after 500ms. This might have caused spurious timeouts! New default timeout is 1500ms. See **network-timeout** setting for more details. Code in [commit 1402](http://wiki.powerdns.com/projects/trac/changeset/1402).

# Recursor version 3.1.7.2
Released on the 6th of January 2010.

This release consist of a number of vital security updates. These updates address issues that can in all likelihood lead to a full system compromise. In addition, it is possible for third parties to pollute your cache with dangerous data, exposing your users to possible harm.

This version has been well tested, and at the time of this release is already powering millions of internet connections, and should therefore be a risk-free upgrade from 3.1.7.1 or any earlier version of the PowerDNS Recursor.

All known versions of the PowerDNS Recursor are impacted to a greater or lesser extent, so an immediate update is advised.

These vulnerabilities were discovered by a third party that can't yet be named, but who we thank for their contribution to a more secure PowerDNS Recursor.

For more information, see [PowerDNS Security Advisory 2010-01](security/powerdns-advisory-2010-01.md "PowerDNS Security Advisory 2010-01: PowerDNS Recursor up to and including 3.1.7.1 can be brought down and probably exploited") and [PowerDNS Security Advisory 2010-02](security/powerdns-advisory-2010-02.md "PowerDNS Security Advisory 2010-02: PowerDNS Recursor up to and including 3.1.7.1 can be spoofed into accepting bogus data").

# Recursor version 3.1.7.1
Released on the 2nd of August 2009.

This release consists entirely of fixes for tiny bugs that have been reported over the past year. In addition, compatibility has been restored with the latest versions of the gcc compiler and the 'boost' libraries.

No features have been added, but some debugging code that very slightly impacted performance (and polluted the console when operating in the foreground) has been removed.

FreeBSD users may want to upgrade because of a very remote chance of 3.1.7 and previous crashing once every few years. For other operators not currently experiencing problems, there is no reason to upgrade.

-   Improved error messages when parsing zones for authoritative serving ([commit 1235](http://wiki.powerdns.com/projects/trac/changeset/1235)).
-   Better resilience against whitespace in configuration (changesets [1237](http://wiki.powerdns.com/projects/trac/changeset/1237), [1240](http://wiki.powerdns.com/projects/trac/changeset/1240), [1242](http://wiki.powerdns.com/projects/trac/changeset/1242))
-   Slight performance increase ([commit 1378](http://wiki.powerdns.com/projects/trac/changeset/1378))
-   Fix rare case where timeouts were not being reported to the right query-thread ([commit 1260](http://wiki.powerdns.com/projects/trac/changeset/1260))
-   Fix compilation against newer versions of the Boost C++ libraries ([commit 1381](http://wiki.powerdns.com/projects/trac/changeset/1381))
-   Close very rare issue with TCP/IP close reporting ECONNRESET on FreeBSD. Reported by Andrei Poelov in [ticket 192](https://github.com/PowerDNS/pdns/issues/192).
-   Silence debugging output ([commit 1286](http://wiki.powerdns.com/projects/trac/changeset/1286)).
-   Fix compilation against newer versions of gcc ([commit 1384](http://wiki.powerdns.com/projects/trac/changeset/1384))
-   No longer set export-etc-hosts to 'on' on reload-zones. Discovered by Paul Cairney, closes [ticket 225](https://github.com/PowerDNS/pdns/issues/225).
-   Sane default for the maximum cache size in the Recursor, suggested by Roel van der Made ([commit 1354](http://wiki.powerdns.com/projects/trac/changeset/1354)).
-   No longer exit because of the changed behaviour of the Solaris 'completion ports' in more recent versions of Solaris. Fix in [commit 1372](http://wiki.powerdns.com/projects/trac/changeset/1372), reported by Jan Gyselinck.

# Authoritative Server version 2.9.22
**Warning**: The 2.9.22.x series of releases is end-of-life and unsupported. It contains many issues and potential security problems. We urge you to upgrade to a recent version of PowerDNS!

Released on the 27th of January 2009.

This is a huge release, spanning almost 20 months of development. Besides fixing a lot of bugs, of note is the addition of the so called 'Notification Proxy', which allows PowerDNS to function as a master server behind a firewall, plus the huge performance improvement of the internal caches.

This work has been made possible by UPC Broadband and Directi, respectively.

Finally, the release candidates of this version have been tested & improved by Jorn Ekkelenkamp, Ton van Rosmalen, Jeff Sipek, Tyler Hall, Christof Meerwald and Stefan Schmidt.

## Fixed between rc1 and rc2, but not an issue in 2.9.21.
-   **pdns\_control ccounts** again outputs proper cache statistics. Implemented in [commit 1304](http://wiki.powerdns.com/projects/trac/changeset/1304).
-   Negative query caching was reinstated, leading to 6 times fewer backend queries than rc1 on the Express.powerdns.com servers.
-   Packetcache no longer needlessly parses outgoing packets before sending them.
-   Fancy records work again. This work has been sponsored by ISP Services. Implemented in [commit 1302](http://wiki.powerdns.com/projects/trac/changeset/1302) and [commit 1299](http://wiki.powerdns.com/projects/trac/changeset/1299).

## New features
-   **pdns\_control** can now also work over TCP/IP. Sponsored by Directi. Commits [1246](http://wiki.powerdns.com/projects/trac/changeset/1246), [1251](http://wiki.powerdns.com/projects/trac/changeset/1251), [1254](http://wiki.powerdns.com/projects/trac/changeset/1254), [1255](http://wiki.powerdns.com/projects/trac/changeset/1255).
-   Implemented a notification proxy, see ["Notification proxy (nproxy)"](tools/analysis.md#nproxy"). This work was sponsored by UPC Broadband. Implemented in commits [1075](http://wiki.powerdns.com/projects/trac/changeset/1075), [1077](http://wiki.powerdns.com/projects/trac/changeset/1077), [1082](http://wiki.powerdns.com/projects/trac/changeset/1082), [1083](http://wiki.powerdns.com/projects/trac/changeset/1083), [1085](http://wiki.powerdns.com/projects/trac/changeset/1085) and [1086](http://wiki.powerdns.com/projects/trac/changeset/1086).
-   IXFR queries are now supported in the sense that we treat them as AXFR queries, silencing warnings in other nameservers. Suggested in [ticket 131](https://github.com/PowerDNS/pdns/issues/131).
-   The PIPE backend has been extended by David Apgar to allow the reporting of errors using the 'FAIL' command, plus support for responses with whitespace. Implemented in [commit 1114](http://wiki.powerdns.com/projects/trac/changeset/1114).
-   PowerDNS Authoritative server now parses incoming EDNS options, like maximum allowed packet size. Implemented in [commit 1123](http://wiki.powerdns.com/projects/trac/changeset/1123) and [commit 1281](http://wiki.powerdns.com/projects/trac/changeset/1281).
-   Added support for DHCID, IPSECKEY and KX records, thanks Norbert Sendetzky for the hint. Implemented in [commit 1144](http://wiki.powerdns.com/projects/trac/changeset/1144).
-   Norbert Sendetzky has has added support for all record types supported by PowerDNS to the LDAPBackend. Furthermore, the detection of OpenLDAP in autoconf has been improved. Finally, debian has supplied some fixes to PowerLDAP. Implemented in [commit 1152](http://wiki.powerdns.com/projects/trac/changeset/1152) and [commit 1153](http://wiki.powerdns.com/projects/trac/changeset/1153).
-   Implemented EDNS NSID option for retrieving the nameserver ID out of band. Defaults to hostname, can be specified using the **server-id** setting. Code in [commit 1232](http://wiki.powerdns.com/projects/trac/changeset/1232).
-   Implemented experimental EDNS PING for enhanced forgery resilience. Code in [commit 1232](http://wiki.powerdns.com/projects/trac/changeset/1232).

## Performance
-   Improve packet generation performance, in some cases by 25%. Code in [1258](http://wiki.powerdns.com/projects/trac/changeset/1258), [1259](http://wiki.powerdns.com/projects/trac/changeset/1259).
-   Improved access list checking performance. [commit 1261](http://wiki.powerdns.com/projects/trac/changeset/1261).
-   PowerDNS Authoritative caches were completely redone, and are now based on the same cache that is in the resolver. This work has been sponsored by Directi. In large benchmarks, PowerDNS performance has improved by an order of magnitude or more. This new version allows for near-instantaneous cache purging, plus very rapid purging based on suffix. Purge commands can also be batched. This work is partially based on an innovative reverse-string comparison function authored by Aki Tuomi.
-   Installations which run with very high cache hitrates can now benefit from multiple CPUs by setting **receiver-threads** to the number of desired CPUs to utilize in cache operations. Implemented in [commit 1316](http://wiki.powerdns.com/projects/trac/changeset/1316).
-   BIND backend speedups in [commit 1108](http://wiki.powerdns.com/projects/trac/changeset/1108), measured at around a 20% improvement, possibly more on very large setups.

## Bugs fixed
-   Tyler Hall discovered the PowerDNS configuration file parser had problems with trailing tabs. This turned out to be a wider problem in PowerDNS. Buggy code replaced by a library call in [commit 1237](http://wiki.powerdns.com/projects/trac/changeset/1237) and [commit 1240](http://wiki.powerdns.com/projects/trac/changeset/1240).
-   David Apgar of Yahoo discovered that our 'guardian' method of restarting PowerDNS in case of problems was not fool proof, and submitted a fix. A variation of this fix can be found in [commit 1323](http://wiki.powerdns.com/projects/trac/changeset/1323). Also reported by Directi.
-   Connection reset by peer events in the TCP nameserver no longer lead to the cycling of database connections. Code in [commit 1241](http://wiki.powerdns.com/projects/trac/changeset/1241).
-   FreeBSD compilation with Generic PostgreSQL backend was fixed. Reported by Wouter de Jong of WideXS, fixed in [commit 1305](http://wiki.powerdns.com/projects/trac/changeset/1305), closes [ticket 95](https://github.com/PowerDNS/pdns/issues/95).
-   Webserver no longer prints '1e2%'. Finally closes [ticket 26](https://github.com/PowerDNS/pdns/issues/26). Much friendly nagging for over 3 years by Jeff Sipek, code in [commit 1303](http://wiki.powerdns.com/projects/trac/changeset/1303).
-   PowerDNS used to ignore certain queries it could not answer. These queries are no longer ignored, but get a SERVFAIL response. Implemented in [commit 1239](http://wiki.powerdns.com/projects/trac/changeset/1239).
-   Fix subtle CNAME and wildcard interactions reported by 'zzyzz', implemented in [commit 1147](http://wiki.powerdns.com/projects/trac/changeset/1147).
-   The generic backends did not honour the **default-ttl** setting. Spotted and implemented by Matti Hiljanen.
-   Matti Hiljanen discovered that the OpenDBX backend did not fill out the SOA ttl value properly. Matti also improved the SQL statements for better compatibility. Implemented in [commit 1181](http://wiki.powerdns.com/projects/trac/changeset/1181).
-   Treat invalid WWW requests better. Spotted by Maikel Verheijen, implemented in [commit 1092](http://wiki.powerdns.com/projects/trac/changeset/1092).
-   Documentation errors and typos, spotted by Marco Davids ([commit 1097](http://wiki.powerdns.com/projects/trac/changeset/1097)) and Rejo Zengers ([commit 1119](http://wiki.powerdns.com/projects/trac/changeset/1119))
-   Properly fill out the 'recursion available'-flag. Spotted by Augie Schwer in [ticket 167](https://github.com/PowerDNS/pdns/issues/167).
-   Several memory leaks on bad data in the database or other errors have been fixed. Addressed in [1078](http://wiki.powerdns.com/projects/trac/changeset/1078) and [1079](http://wiki.powerdns.com/projects/trac/changeset/1079).
-   In contravention to the documentation, the domain type as specified in the database ('MASTER', 'SLAVE' or 'NATIVE') was interpreted case sensitively. [1084](http://wiki.powerdns.com/projects/trac/changeset/1084).
-   BIND backend could crash on processing information about slave zones to be checked. Spotted by Stefan Schmidt, fixed in [1089](http://wiki.powerdns.com/projects/trac/changeset/1089).
-   Jelte Jansen of Stichting NLNetLabs discovered PowerDNS in BIND mode couldn't operate as a root-server! Fixed in [1057](http://wiki.powerdns.com/projects/trac/changeset/1057).
-   'DPS' discovered there was a rare opportunity for PowerDNS to lock up waiting for new data. Addressed in [1076](http://wiki.powerdns.com/projects/trac/changeset/1076).
-   Make singlethreaded mode more resilient against errors. [commit 1272](http://wiki.powerdns.com/projects/trac/changeset/1272).
-   DNSSEC records were part of 2.9.21, but were not actually hooked up. Please note that while PowerDNS can serve most DNSSEC records, it does not do DNSSEC processing. Implemented in [1046](http://wiki.powerdns.com/projects/trac/changeset/1046).
-   Shawn Starr migrated all his domains to PowerDNS in one evening, from an installation that had been used since BIND4. In doing so, he found 3 bugs in as many hours. An **IN** statement in the BIND `named.conf` with a zone with a trailing dot was misparsed, fixed in [commit 1233](http://wiki.powerdns.com/projects/trac/changeset/1233). Secondly, the zone file parser tripped over a line consisting of nothing but comments in the wrong place. Finally '$ORIGIN .' was misparsed. Last two issues fixed in [commit 1234](http://wiki.powerdns.com/projects/trac/changeset/1234).
-   Our statistics counters did not wrap correctly after the 2.15 billion mark. Spotted by Stefan Schmidt, reported in [ticket 179](https://github.com/PowerDNS/pdns/issues/179), fixed in [commit 1284](http://wiki.powerdns.com/projects/trac/changeset/1284).
-   Bindbackend could sometimes generate very strange error messages while processing a malformed zone file. Sometimes such error messages could cause a crash (reported on HP-UX). Addressed by [commit 1279](http://wiki.powerdns.com/projects/trac/changeset/1279). This could not be triggered remotely. Closes ticket [ticket 203](https://github.com/PowerDNS/pdns/issues/203).
-   Pipe backend did not clean up killed coprocesses. Found and fixed by Daniel Drown
-   Installations with tens of thousands of slave domains would never complete the cycle to check the freshness of all zones as each incoming notification disrupted this cycle. Addressed in cooperation with Tyler Hall of EditDNS.

## Improvements
-   Zone parser improvements mean $TTL and $INCLUDES now work a lot better. Implemented in [1056](http://wiki.powerdns.com/projects/trac/changeset/1056), [1062](http://wiki.powerdns.com/projects/trac/changeset/1062).
-   No longer report temporary recvfrom errors, which used to spam the log on many systems. Addressed in [commit 1320](http://wiki.powerdns.com/projects/trac/changeset/1320).
-   Direct queries for 'fancy records' would lead to errors, such queries now fail early. Spotted by Jorn Ekkelenkamp, implemented in [1051](http://wiki.powerdns.com/projects/trac/changeset/1051).
-   Fix typo in geobackend, closing [ticket 157](https://github.com/PowerDNS/pdns/issues/157), implemented in [1090](http://wiki.powerdns.com/projects/trac/changeset/1090).
-   Initial work on TSIG support - not done yet. Spurred on by Marco Davids.
-   Embarrassingly, the 'master' configuration setting was not documented in the list of all settings!
-   Norbert has updated OpenDBX so that SQLite reads and writes no longer deadlock, plus compilation fixes on Solaris, plus the addition of autoserials to backends that support triggers. Implemented in [commit 1154](http://wiki.powerdns.com/projects/trac/changeset/1154).
-   Random generator is now based on AES, improving the security of certain proxy operations. This is the same random generator that is in the recursor. Implemented in [commit 1256](http://wiki.powerdns.com/projects/trac/changeset/1256).
-   Documentation for 'supermaster' mode was improved due to popular demand.
-   When binding to a UDP port failed, supply a more precise error message ([commit 1245](http://wiki.powerdns.com/projects/trac/changeset/1245))
-   The zone parser error messages were vastly improved, partially inspired by Shawn's cowboy migration. Code in [commit 1235](http://wiki.powerdns.com/projects/trac/changeset/1235).
-   Labels are compressed more efficiently (case-insensitively), leading to smaller packets. Implemented in [commit 1156](http://wiki.powerdns.com/projects/trac/changeset/1156).
-   Fix handling of TCP timeouts to not cause a reload of the backends. Implemented in [commit 1092](http://wiki.powerdns.com/projects/trac/changeset/1092).
-   TCP Receiver no longer spams the log with common network errors. Implemented in [commit 1306](http://wiki.powerdns.com/projects/trac/changeset/1306).
-   Move from select() to poll()-based multiplexing, allowing PowerDNS to listen on more than 1024 sockets simultaneously. One big PowerDNS user needs this. Implemented in [1072](http://wiki.powerdns.com/projects/trac/changeset/1072).
-   Zone2sql now reads source files in performance enhancing inode order. Additionally, zone2sql no longer dies on a missing zone file if **--on-error-resume-next** was specified. Finally, statistics of zone2sql conversion have been improved. Implemented in [1055](http://wiki.powerdns.com/projects/trac/changeset/1055).
-   Address issues found by more recent g++ versions. Spotted and/or fixed by Jorn Ekkelenkamp ([commit 1051](http://wiki.powerdns.com/projects/trac/changeset/1051)), Marcus Rueckert ([commit 1094](http://wiki.powerdns.com/projects/trac/changeset/1094)), Norbert Sendetzky ([commit 1107](http://wiki.powerdns.com/projects/trac/changeset/1107)), Serge Belyshev ([commit 1171](http://wiki.powerdns.com/projects/trac/changeset/1171)).
-   The Intel C Compiler implements certain things differently, causing the master/slave communicator to malfunction. Spotted by Marcus Rueckert, implemented in [1052](http://wiki.powerdns.com/projects/trac/changeset/1052), plus fallout in [1105](http://wiki.powerdns.com/projects/trac/changeset/1105).
-   PowerDNS can now be compiled with Boost 1.37.0.
-   Andre Lorbach of Adiscon discovered the Microsoft Windows 2003 nameserver adds out of zone data to zone transfers, which we need to ignore, instead of rejecting the entire zone. Implemented in [1048](http://wiki.powerdns.com/projects/trac/changeset/1048).
-   PowerDNS now skips remote master servers which consistently generate timeout messages, improving the master checking cycle time tremendously. Developed in cooperation with Tyler Hall. Implemented in [commit 1278](http://wiki.powerdns.com/projects/trac/changeset/1278).
-   When binding to a UDP port failed, supply a more precise error message ([commit 1245](http://wiki.powerdns.com/projects/trac/changeset/1245))
-   **dnsreplay** now waits for the final answers to arrive, making it possible to process even small pcap files and get meaningful statistics. [commit 1268](http://wiki.powerdns.com/projects/trac/changeset/1268).
-   **dnsreplay** has a more sane default timeout now, which can be configured too. Suggested by Augie Schwer in [ticket 163](https://github.com/PowerDNS/pdns/issues/163), implemented in [commit 1287](http://wiki.powerdns.com/projects/trac/changeset/1287).

# Authoritative Server version 2.9.21.2
Released on the 18th of November 2008.

This release consists of a single patch to PowerDNS Authoritative Server version 2.9.21.1. In some configurations, notably with configuration option 'distributor-threads=1', the PowerDNS Authoritative Server crashes easily in some error conditions.

All users are urged to upgrade. Even though PowerDNS restarts itself on encountering such error conditions, and even though most PowerDNS configurations do not run in single threaded mode, an upgrade is recommended.

More detail can be found in [PowerDNS Security Advisory 2008-02](security/powerdns-advisory-2008-03.md "PowerDNS Security Advisory 2008-02: Some PowerDNS Configurations can be forced to restart remotely").

# Authoritative Server version 2.9.21.1
Released on the 6th of August 2008.

This release consists of a single patch to PowerDNS Authoritative Server version 2.9.21. Brian J. Dowling of Simplicity Communications has discovered a security implication of the previous PowerDNS behaviour to drop queries it considers malformed. We are grateful that Brian notified us quickly about this problem.

This issue has been assigned CVE-2008-3337. The single patch is in [commit 1239](http://wiki.powerdns.com/projects/trac/changeset/1239). More detail can be found in [PowerDNS Security Advisory 2008-02](security/powerdns-advisory-2008-02.md "PowerDNS Security Advisory 2008-02: By not responding to certain queries, domains become easier to spoof").

The implication is that while the PowerDNS Authoritative server itself does not face a security risk because of dropping these malformed queries, other resolving nameservers run a higher risk of accepting spoofed answers for domains being hosted by PowerDNS Authoritative Servers before 2.9.21.1.

While the dropping of queries does not aid sophisticated spoofing attempts, it does facilitate simpler attacks.

It may be good to know that several large sites already run with this patch applied, as it has been in the public code base for some weeks already.

# Recursor version 3.1.7
Released the 25th of June 2008.

This version contains powerful scripting abilities, allowing operators to modify DNS responses in many interesting ways. Among other things, these abilities can be used to filter out malware domains, to perform load balancing, to comply with legal and other requirements and finally, to implement 'NXDOMAIN' redirection.

It is hoped that the addition of Lua scripting will enable responsible DNS modification for those that need it.

For more details about the Lua scripting, which can be modified, loaded and unloaded at runtime, see [Scripting](recursor/scripting.md "Scripting"). Many thanks are due to the \#lua irc channel, for excellent near-realtime Lua support. In addition, a number of PowerDNS users have been enthusiastically testing prereleases of the scripting support, and have found and solved many issues.

In addition, 3.1.7 fixes a number of bugs

-   In 3.1.5 and 3.1.6, an authoritative server could continue to renew its authority, even though a domain had been delegated to other servers in the meantime.

    In the rare cases where this happened, and the old servers were not shut down, the observed effect is that users were fed outdated data. Bug spotted and analysed by Darren Gamble, fix in [commit 1182](http://wiki.powerdns.com/projects/trac/changeset/1182) and [commit 1183](http://wiki.powerdns.com/projects/trac/changeset/1183).

-   Thanks to long time PowerDNS contributor Stefan Arentz, for the first time, Mac OS X 10.5 users can compile and run the PowerDNS Recursor! Patch in [commit 1185](http://wiki.powerdns.com/projects/trac/changeset/1185).
-   Sten Spans spotted that for outgoing TCP/IP queries, the **query-local-address** setting was not honored. Fixed in [commit 1190](http://wiki.powerdns.com/projects/trac/changeset/1190).
-   **rec\_control wipe-cache** now also wipes domains from the negative cache, hurrying up the expiry of negatively cached records. Suggested by Simon Kirby, implemented in [commit 1204](http://wiki.powerdns.com/projects/trac/changeset/1204).
-   When a forwarder server is configured for a domain, using the **forward-zones** setting, this server IP address was filtered using the **dont-query** setting, which is generally not what is desired: the server to which queries are forwarded will often live in private IP space, and the operator should be trusted to know what he is doing. Reported and argued by Simon Kirby, fix in [commit 1211](http://wiki.powerdns.com/projects/trac/changeset/1211).
-   Marcus Rueckert of OpenSUSE reported that very recent gcc versions emitted a (correct) warning on an overly complicated line in syncres.cc, fixed in [commit 1189](http://wiki.powerdns.com/projects/trac/changeset/1189).
-   Stefan Schmidt discovered that the netmask matching code, used by the new Lua scripts, but also by all other parts of PowerDNS, had problems with explicit '/32' matches. Fixed in [commit 1205](http://wiki.powerdns.com/projects/trac/changeset/1205).

# Recursor version 3.1.6
Released on the 1st of May 2008.

This version fixes two important problems, each on its own important enough to justify a quick upgrade.

-   Version 3.1.5 had problems resolving several slightly misconfigured domains, including for a time 'juniper.net'. Nameserver timeouts were not being processed correctly, leading PowerDNS to not update the internal clock, which in turn meant that any queries immediately following an error would time out as well. Because of retries, this would usually not be a problem except on very busy servers, for domains with different nameservers at different levels of the DNS-hierarchy, like 'juniper.net'.

    This issue was fixed rapidly because of the help of [XS4ALL](http://www.xs4all.nl) (Eric Veldhuyzen, Kai Storbeck), Brad Dameron and Kees Monshouwer. Fix in [commit 1178](http://wiki.powerdns.com/projects/trac/changeset/1178).

-   The new high-quality random generator was not used for all random numbers, especially in source port selection. This means that 3.1.5 is still a lot more secure than 3.1.4 was, and its algorithms more secure than most other nameservers, but it also means 3.1.5 is not as secure as it could be. A quick upgrade is recommended. Discovered by Thomas Biege of Novell (SUSE), fixed in [commit 1179](http://wiki.powerdns.com/projects/trac/changeset/1179).

# Recursor version 3.1.5
Released on the 31st of March 2008.

Much like 3.1.4, this release does not add a lot of major features. Instead, performance has been improved significantly (estimated at around 20%), and many rare and not so rare issues were addressed. Multi-part TXT records now work as expected - the only significant functional bug found in 15 months. One of the oldest feature requests was fulfilled: version 3.1.5 can finally forward queries for designated domains to multiple servers, on differing port numbers if needed. Previously only one forwarder address was supported. This lack held back a number of migrations to PowerDNS.

We would like to thank Amit Klein of Trusteer for bringing a serious vulnerability to our attention which would enable a smart attacker to 'spoof' previous versions of the PowerDNS Recursor into accepting possibly malicious data.

Details can be found on [this Trusteer page](http://www.trusteer.com/docs/powerdnsrecursor.html).

It is recommended that all users of the PowerDNS Recursor upgrade to 3.1.5 as soon as practicable, while we simultaneously note that busy servers are less susceptible to the attack, but not immune.

The PowerDNS Security Advisory can be found in [PowerDNS Security Advisory 2008-01](security/powerdns-advisory-2008-01.md "PowerDNS Security Advisory 2008-01: System random generator can be predicted, leading to the potential to 'spoof' PowerDNS Recursor").

This version can properly benefit from all IPv4 and IPv6 addresses in use at the root-servers as of early February 2008. In order to implement this, changes were made to how the Recursor deals internally with A and AAAA queries for nameservers, see below for more details.

Additionally, newer releases of the G++ compiler required some fixes (see [ticket 173](https://github.com/PowerDNS/pdns/issues/173)).

This release was made possible by the help of Wichert Akkerman, Winfried Angele, Arnoud Bakker (Fox-IT), Niels Bakker (no relation!), Leo Baltus (Nederlandse Publieke Omroep), Marco Davids (SIDN), David Gavarret (Neuf Cegetel), Peter Gervai, Marcus Goller (UPC), Matti Hiljanen (Saunalahti/Elisa), Ruben Kerkhof, Alex Kiernan, Amit Klein (Trusteer), Kenneth Marshall (Rice University), Thomas Rietz, Marcus Rueckert (OpenSUSE), Augie Schwer (Sonix), Sten Spans (Bit), Stefan Schmidt (Freenet), Kai Storbeck (xs4all), Alex Trull, Andrew Turnbull (No Wires) and Aaron Thompson, and many more who filed bugs anonymously, or who we forgot to mention.

## Security related issues
-   Amit Klein has informed us that System random generator output can be predicted based on its past behaviour, allowing a smart attacker to 'spoof' our nameserver. Full details in [PowerDNS Security Advisory 2008-01](security/powerdns-advisory-2008-01.md "PowerDNS Security Advisory 2008-01: System random generator can be predicted, leading to the potential to 'spoof' PowerDNS Recursor").
-   The Recursor will by default no longer query private-space nameservers. This closes a slight security risk and simultaneously improves performance and stability. For more information, see **dont-query** in [pdns\_recursor settings](recursor/settings.md#dont-query "pdns_recursor settings"). Implemented in [commit 923](http://wiki.powerdns.com/projects/trac/changeset/923).
-   Applied fix for [ticket 110](https://github.com/PowerDNS/pdns/issues/110) ('PowerDNS should change directory to '/' in chroot), implemented in [commit 944](http://wiki.powerdns.com/projects/trac/changeset/944).

## Performance
-   The DNS packet writing and parsing infrastructure performance was improved in several ways, see commits [925](http://wiki.powerdns.com/projects/trac/changeset/925), [926](http://wiki.powerdns.com/projects/trac/changeset/926), [928](http://wiki.powerdns.com/projects/trac/changeset/928), [931](http://wiki.powerdns.com/projects/trac/changeset/931), [1021](http://wiki.powerdns.com/projects/trac/changeset/1021), [1050](http://wiki.powerdns.com/projects/trac/changeset/1050).
-   Remove multithreading overhead from the Recursor ([commit 999](http://wiki.powerdns.com/projects/trac/changeset/999)).

## Bug fixes
-   Built-in authoritative server now properly derives the TTL from the SOA record if not specified. Implemented in [commit 1165](http://wiki.powerdns.com/projects/trac/changeset/1165). Additionally, even when TTL was specified for the built-in authoritative server, it was ignored. Reported by Stefan Schmidt, closing [ticket 147](https://github.com/PowerDNS/pdns/issues/147).
-   Empty TXT record components can now be served. Implemented in [commit 1166](http://wiki.powerdns.com/projects/trac/changeset/1166), closing [ticket 178](https://github.com/PowerDNS/pdns/issues/178). Spotted by Matti Hiljanen.
-   The Recursor would not properly override old data with new, sometimes serving old and new data concurrently. Fixed in [commit 1137](http://wiki.powerdns.com/projects/trac/changeset/1137).
-   SOA records with embedded carriage-return characters are now parsed correctly. Implemented in [commit 1167](http://wiki.powerdns.com/projects/trac/changeset/1167), closing [ticket 162](https://github.com/PowerDNS/pdns/issues/162).
-   Some routing conditions could cause UDP connected sockets to generate an error which PowerDNS did not deal with properly, leading to a leaked file descriptor. As these run out over time, the recursor could crash. This would also happen for IPv6 queries on a host with no IPv6 connectivity. Thanks to Kai of xs4all and Wichert Akkerman for reporting this issue. Fix in [commit 1133](http://wiki.powerdns.com/projects/trac/changeset/1133).
-   Empty unknown record types can now be stored without generating a scary error ([commit 1129](http://wiki.powerdns.com/projects/trac/changeset/1129))
-   Applied fix for [ticket 111](https://github.com/PowerDNS/pdns/issues/111), [ticket 112](https://github.com/PowerDNS/pdns/issues/112) and [ticket 153](https://github.com/PowerDNS/pdns/issues/153) - large (multipart) TXT records are now retrieved and served properly. Fix in [commit 996](http://wiki.powerdns.com/projects/trac/changeset/996).
-   Solaris compilation instructions in Recursor documentation were wrong, leading to an instant crash on startup. Luckily nobody reads the documentation, except for Marcus Goller who found the error. Fixed in [commit 1124](http://wiki.powerdns.com/projects/trac/changeset/1124).
-   On Solaris, finally fix the issue where queries get distributed strangely over CPUs, or not get distributed at all. Much debugging and analysing performed by Alex Kiernan, who also supplied fixes. Implemented in [commit 1091](http://wiki.powerdns.com/projects/trac/changeset/1091), [commit 1093](http://wiki.powerdns.com/projects/trac/changeset/1093).
-   Various fixes for modern G++ versions, most spotted by Marcus Rueckert (commits [964](http://wiki.powerdns.com/projects/trac/changeset/964), [965](http://wiki.powerdns.com/projects/trac/changeset/965), [1028](http://wiki.powerdns.com/projects/trac/changeset/1028), [1052](http://wiki.powerdns.com/projects/trac/changeset/1052)), and Ruben Kerkhof ([commit 1136](http://wiki.powerdns.com/projects/trac/changeset/1136), closing [ticket 175](https://github.com/PowerDNS/pdns/issues/175)).
-   Recursor would not properly clean up pidfile and control socket, closing [ticket 120](https://github.com/PowerDNS/pdns/issues/120), code in [commit 988](http://wiki.powerdns.com/projects/trac/changeset/988), [commit 1098](http://wiki.powerdns.com/projects/trac/changeset/1098) (part of fix by Matti Hiljanen, spotted by Leo Baltus)
-   Recursor can now serve multi-line records from its limited authoritative server ([commit 1014](http://wiki.powerdns.com/projects/trac/changeset/1014)).
-   When parsing zones, the 'm' time specification stands for minutes, not months! Closing Debian bug 406462 ([commit 1026](http://wiki.powerdns.com/projects/trac/changeset/1026))
-   Authoritative zone parser did not support '@' in the content of records. Spotted by Marco Davids, fixed in [commit 1030](http://wiki.powerdns.com/projects/trac/changeset/1030).
-   Authoritative zone parser could be confused by trailing TABs on record lines ([commit 1062](http://wiki.powerdns.com/projects/trac/changeset/1062)).
-   EINTR error code could block entire server if received at the wrong time. Spotted by Arnoud Bakker, fix in [commit 1059](http://wiki.powerdns.com/projects/trac/changeset/1059).
-   Fix crash on NetBSD on Alpha CPUs, might improve startup behaviour on empty caches on other architectures as well ([commit 1061](http://wiki.powerdns.com/projects/trac/changeset/1061)).
-   Outbound TCP queries were being performed sub-optimally because of an interaction with the 'MPlexer'. Fixes in [commit 1115](http://wiki.powerdns.com/projects/trac/changeset/1115), [commit 1116](http://wiki.powerdns.com/projects/trac/changeset/1116).

## New features
-   Implemented **rec\_control** command **get uptime**, as suggested by Niels Bakker ([commit 935](http://wiki.powerdns.com/projects/trac/changeset/935)). Added to default rrdtool scripts in [commit 940](http://wiki.powerdns.com/projects/trac/changeset/940).
-   The Recursor Authoritative component, meant for having the Recursor serve some zones authoritatively, now supports $INCLUDE and $GENERATE. Implemented in [commit 951](http://wiki.powerdns.com/projects/trac/changeset/951) and [commit 952](http://wiki.powerdns.com/projects/trac/changeset/952), [commit 967](http://wiki.powerdns.com/projects/trac/changeset/967) (discovered by Thomas Rietz),
-   Implemented **forward-zones-file** option in order to support larger amounts of zones which should be forwarded to another nameserver ([commit 963](http://wiki.powerdns.com/projects/trac/changeset/963)).
-   Both **forward-zones** and **forward-zones-file** can now specify multiple forwarders per domain, implemented in [commit 1168](http://wiki.powerdns.com/projects/trac/changeset/1168), closing [ticket 81](https://github.com/PowerDNS/pdns/issues/81). Additionally, both these settings can also specify non-standard port numbers, as suggested in ticket [ticket 122](https://github.com/PowerDNS/pdns/issues/122). Patch authored by Aaron Thompson, with additional work by Augie Schwer.
-   Sten Spans contributed **allow-from-file**, implemented in [commit 1150](http://wiki.powerdns.com/projects/trac/changeset/1150). This feature allows the Recursor to read access rules from a (large) file.

## General improvements
-   Ruben Kerkhof fixed up weird permission bits as well as our SGML documentation code in [commit 936](http://wiki.powerdns.com/projects/trac/changeset/936) and [commit 937](http://wiki.powerdns.com/projects/trac/changeset/937).
-   Full IPv6 parity. If configured to use IPv6 for outgoing queries (using **query-local-address6=::0** for example), IPv6 and IPv4 addresses are finally treated 100% identically, instead of 'mostly'. This feature is implemented using 'ANY' queries to find A and AAAA addresses in one query, which is a new approach. Treat with caution.
-   Now perform EDNS0 root refreshing queries, so as to benefit from all returned addresses. Relevant since early February 2008 when the root-servers started to respond with IPv6 addresses, which made the default non-EDNS0 maximum packet length reply no longer contain all records. Implemented in [commit 1130](http://wiki.powerdns.com/projects/trac/changeset/1130). Thanks to dns-operations AT mail.oarc.isc.org for quick suggestions on how to deal with this change.
-   **rec\_control** now has a timeout in case the Recursor does not respond. Implemented in [commit 945](http://wiki.powerdns.com/projects/trac/changeset/945).
-   (Error) messages are now logged with saner priorities ([commit 955](http://wiki.powerdns.com/projects/trac/changeset/955)).
-   Outbound query IP interface stemmed from 1997 (!) and was in dire need of a cleanup ([commit 1117](http://wiki.powerdns.com/projects/trac/changeset/1117)).
-   L.ROOT-SERVERS.NET moved ([commit 1118](http://wiki.powerdns.com/projects/trac/changeset/1118)).

# PowerDNS Authoritative Server version 2.9.21
Released the 21st of April 2007.

This is the first release the PowerDNS Authoritative Server since the Recursor was split off to a separate product, and also marks the transfer of the new technology developed specifically for the recursor, back to the authoritative server.

This move has reduced the amount of code of the Authoritative server by over 2000 lines, while improving the quality of the program enormously.

However, since so much has been changed, care should be taken when deploying 2.9.21.

To signify the magnitude of the underlying improvements, the next release of the PowerDNS Authoritative Server will be called 3.0.

This release would not have been possible without large amounts of help and support from the PowerDNS Community. We specifically want to thank Massimo Bandinelli of Italy's [Register.it](http://register.it), [Dave Aaldering of Aaldering ICT](http://aaldering-ict.nl), [True BV](http://true.nl), [XS4ALL](http://www.xs4all.nl), Daniel Bilik of [Neosystem](http://www.neosystem.cz), [EasyDNS](http://www.easydns.com), [Heinrich Ruthensteiner](http://www.siemens.com) of Siemens, [Augie Schwer](http://schwer.us), [Mark Bergsma](http://www.wikipedia.org), [Marco Davids](http://www.forfun.net), [Marcus Rueckert of OpenSUSE](http://www.opensuse.org), Andre Muraro of [Locaweb](http://www.locaweb.com.br), Antony Lesuisse, [Norbert Sendetzky](http://www.linuxnetworks.de), [Marco Chiavacci](http://www.aruba.it), Christoph Haas, Ralf van der Enden and Ruben Kerkhof.

## Security issues
-   The previous packet parsing and generating code contained no known bugs, but was however very lengthy and overly complex, and might have had security problems. The new code is 'inherently safe' because it relies on bounds-checking C++ constructs. Therefore, a move to 2.9.21 is highly recommended.
-   Pre-2.9.21, communication between master and server nameservers was not checked as rigidly as possible, possibly allowing third parties to disrupt but not modify such communications.

**Warning**: The 'bind1' legacy version of our BIND backend has been dropped! There should be no need to rely on this old version anymore, as the main BIND backend has been very well tested recently.

## Bugs
-   Multi-part TXT records weren't supported. This has been fixed, and regression tests have been added. Code in commits [1016](http://wiki.powerdns.com/projects/trac/changeset/1016), [996](http://wiki.powerdns.com/projects/trac/changeset/996), [994](http://wiki.powerdns.com/projects/trac/changeset/994).
-   Email addresses with embedded dots in SOA records were not parsed correctly, nor were other embedded dots. Noted by 'Bastiaan', fixed in [commit 1026](http://wiki.powerdns.com/projects/trac/changeset/1026).
-   BIND backend treated the 'm' TTL modifier as 'months' and not 'minutes'. Closes Debian bug 406462. Addressed in [commit 1026](http://wiki.powerdns.com/projects/trac/changeset/1026).
-   Our snapshots were built against a static version of PostgreSQL that was incompatible with many Linux distributions, leading to instant crashes on startup. Fixed in [1022](http://wiki.powerdns.com/projects/trac/changeset/1022) and [1023](http://wiki.powerdns.com/projects/trac/changeset/1023).
-   CNAME referrals to child zones gave improper responses. Noted by Augie Schwer in [ticket 123](https://github.com/PowerDNS/pdns/issues/123), fixed in [commit 992](http://wiki.powerdns.com/projects/trac/changeset/992).
-   When passing a port number with the **recursor** setting, this would sometimes generate errors during additional processing. Switched off overly helpful additional processing for recursive queries to remove this problem. Implemented in [commit 1031](http://wiki.powerdns.com/projects/trac/changeset/1031), spotted by Ralf van der Enden.
-   NS to a nameserver with the name of the zone itself generated problems. Spotted by Augie Schwer, fixed in [commit 947](http://wiki.powerdns.com/projects/trac/changeset/947).
-   Multi-line records in the BIND backend were not always parsed correctly. Fixed in [commit 1014](http://wiki.powerdns.com/projects/trac/changeset/1014).
-   The LOC-record had problems operating outside of the eastern hemisphere of the northern part of the world! Fixed in [commit 1011](http://wiki.powerdns.com/projects/trac/changeset/1011).
-   Backends were compiled without multithreading preprocessor flags. As far as we can determine, this would only cause problems for the BIND backend, but we cannot rule out this caused instability in other backends. Fixed in [commit 1001](http://wiki.powerdns.com/projects/trac/changeset/1001).
-   The BIND backend was highly unstable under reloads, and leaked memory and file descriptors. Thanks to Mark Bergsma and Massimo Bandinelli for respectively pointing this out to us and testing large amounts of patches to fix the problem. The fixes have resulted in better performance, less code, and a remarkable simplification of this backend. Commits [1039](http://wiki.powerdns.com/projects/trac/changeset/1039), [1034](http://wiki.powerdns.com/projects/trac/changeset/1034), [1035](http://wiki.powerdns.com/projects/trac/changeset/1035), [1006](http://wiki.powerdns.com/projects/trac/changeset/1006), [999](http://wiki.powerdns.com/projects/trac/changeset/999), [905](http://wiki.powerdns.com/projects/trac/changeset/905) and previous.
-   BIND backend gave convincing NXDOMAINs on unloaded zones in some cases. Spotted and fixed by Daniel Bilik in [commit 984](http://wiki.powerdns.com/projects/trac/changeset/984).
-   SOA records in zone transfers sometimes contained the wrong SOA TTL. Spotted by Christian Kuehn, fixed in [commit 902](http://wiki.powerdns.com/projects/trac/changeset/902).
-   PowerDNS could get confused by very high SOA serial numbers. Spotted and fixed by Dan Bilik, fixed in [commit 626](http://wiki.powerdns.com/projects/trac/changeset/626).
-   Some versions of FreeBSD perform very strict checks on socket address sizes passed to 'connect', which could lead to problems retrieving zones over AXFR. Fixed in [commit 891](http://wiki.powerdns.com/projects/trac/changeset/891).
-   Some versions of FreeBSD perform very strict checks on IPv6 socket addresses, leading to problems. Discovered by Sten Spans, fixed in [commit 885](http://wiki.powerdns.com/projects/trac/changeset/885) and [commit 886](http://wiki.powerdns.com/projects/trac/changeset/886).
-   IXFR requests were not logged properly. Noted by Ralf van der Enden, fixed in [commit 990](http://wiki.powerdns.com/projects/trac/changeset/990).
-   Some NAPTR records needed an additional space character to encode correctly. Spotted by Heinrich Ruthensteiner, fixed in [commit 1029](http://wiki.powerdns.com/projects/trac/changeset/1029).
-   Many bugs in the TCP nameserver, leading to a PowerDNS process that did not respond to TCP queries over time. Many fixes provided by Dan Bilik, other problems were fixed by rewriting our TCP handling code. Commits [982](http://wiki.powerdns.com/projects/trac/changeset/982) and [980](http://wiki.powerdns.com/projects/trac/changeset/980), [950](http://wiki.powerdns.com/projects/trac/changeset/950), [924](http://wiki.powerdns.com/projects/trac/changeset/924), [889](http://wiki.powerdns.com/projects/trac/changeset/889), [874](http://wiki.powerdns.com/projects/trac/changeset/874), [869](http://wiki.powerdns.com/projects/trac/changeset/869), [685](http://wiki.powerdns.com/projects/trac/changeset/685), [684](http://wiki.powerdns.com/projects/trac/changeset/684).
-   Fix crashes on the ARM processor due to alignment errors. Thanks to Sjoerd Simons. Closes Debian bug 397031.
-   Missing data in generic SQL backends would sometimes lead to faked SOA serial data. Spotted by Leander Lakkas from True. Fix in [commit 866](http://wiki.powerdns.com/projects/trac/changeset/866).
-   When receiving two quick notifications in succession, the packet cache would sometimes "process" the second one, leading PowerDNS to ignore it. Spotted by Dan Bilik, fixed in [commit 686](http://wiki.powerdns.com/projects/trac/changeset/686).
-   Geobackend (by Mark Bergsma) did not properly override the getSOA method, breaking non-overlay operation of this fine backend. The geobackend now also skips '.hidden' configuration files, and now properly disregards empty configuration files. Additionally, the overlapping abilities were improved. Details available in [commit 876](http://wiki.powerdns.com/projects/trac/changeset/876), by Mark.

## Features
-   Thanks to [EasyDNS](http://www.easydns.com), PowerDNS now supports multiple masters per domain. For configuration details, see [Slave operation](authoritative/modes-of-operation.md#slave-operation "Slave operation"). Implemented in [commit 1018](http://wiki.powerdns.com/projects/trac/changeset/1018), [commit 1017](http://wiki.powerdns.com/projects/trac/changeset/1017).
-   Thanks to [EasyDNS](http://www.easydns.com), PowerDNS now supports the KEY record type, as well the SPF record. In [commit 976](http://wiki.powerdns.com/projects/trac/changeset/976).
-   Added support for CERT, SSHFP, DNSKEY, DS, NSEC, RRSIG record types, as part of the move to the new DNS parsing/generating code.
-   Support for the AFSDB record type, as requested by 'Bastian'. Implemented in [commit 978](http://wiki.powerdns.com/projects/trac/changeset/978), closing [ticket 129](https://github.com/PowerDNS/pdns/issues/129).
-   Support for the MR record type. Implemented in [commit 941](http://wiki.powerdns.com/projects/trac/changeset/941) and [commit 1019](http://wiki.powerdns.com/projects/trac/changeset/1019).
-   Gsqlite3 backend was added by Antony Lesuisse in [commit 942](http://wiki.powerdns.com/projects/trac/changeset/942);
-   Added the ability to send out light-weight root-referrals that save bandwidth yet still placate mediocre resolver implementations. Implemented in [commit 912](http://wiki.powerdns.com/projects/trac/changeset/912), enable with 'root-referral=lean'.

## Improvements
-   Miscellaneous OpenDBX and LDAP backend improvements by Norbert Sendetzky. Applied in [commit 977](http://wiki.powerdns.com/projects/trac/changeset/977) and [commit 1040](http://wiki.powerdns.com/projects/trac/changeset/1040).
-   SGML source of the documentation was cleaned up by Ruben Kerkhof in [commit 936](http://wiki.powerdns.com/projects/trac/changeset/936).
-   Speedups in core DNS label processing code. Implemented in [commit 928](http://wiki.powerdns.com/projects/trac/changeset/928), [commit 654](http://wiki.powerdns.com/projects/trac/changeset/654), [commit 1020](http://wiki.powerdns.com/projects/trac/changeset/1020).
-   When communicating with master servers and encountering errors, more useful details are logged. Reported by Stefan Arentz in [ticket 137](https://github.com/PowerDNS/pdns/issues/137), closed by [commit 1015](http://wiki.powerdns.com/projects/trac/changeset/1015).
-   Database errors are now logged with more details. Addressed in [commit 1004](http://wiki.powerdns.com/projects/trac/changeset/1004).
-   pdns\_control problems are now logged more verbosely. Change in [commit 910](http://wiki.powerdns.com/projects/trac/changeset/910).
-   Erroneous address configuration was logged unclearly. Spotted by River Tarnell, fixed in [commit 888](http://wiki.powerdns.com/projects/trac/changeset/888).
-   Example configuration shipped with PowerDNS was very old. Noted by Leen Besselink, fixed in [commit 946](http://wiki.powerdns.com/projects/trac/changeset/946).
-   PowerDNS neglected to chdir to the root when chrooted. This closes [ticket 110](https://github.com/PowerDNS/pdns/issues/110), fixed in [commit 944](http://wiki.powerdns.com/projects/trac/changeset/944).
-   Microsoft resolver had problems with responses we generated for CNAMEs pointing out of our bailiwick. Fixed in [commit 983](http://wiki.powerdns.com/projects/trac/changeset/983) and expedited by Locaweb.com.br.
-   Built-in webserver logs errors more verbosely. Closes [ticket 82](https://github.com/PowerDNS/pdns/issues/82), fixed in [commit 991](http://wiki.powerdns.com/projects/trac/changeset/991).
-   Queries containing '@' no longer flood the logs. Addressed in [commit 1014](http://wiki.powerdns.com/projects/trac/changeset/1014).
-   The build process now looks for PostgreSQL in more places. Implemented in [commit 998](http://wiki.powerdns.com/projects/trac/changeset/998), closes [ticket 90](https://github.com/PowerDNS/pdns/issues/90).
-   Speedups in the BIND backend now mean large installations enjoy startup times up to 30 times faster than with the original BIND nameserver. Many thanks to Massimo Bandinelli.
-   BIND backend now offers full support for query logging, implemented in [commit 1026](http://wiki.powerdns.com/projects/trac/changeset/1026), [commit 1029](http://wiki.powerdns.com/projects/trac/changeset/1029).
-   BIND backend named.conf parsing is now fully case-insensitive for domain names. This closes Debian bug 406461, fixed in [commit 1027](http://wiki.powerdns.com/projects/trac/changeset/1027).
-   IPv6 and IPv4 address parsing routines have been replaced, which should result in prettier output in some cases. [commit 962](http://wiki.powerdns.com/projects/trac/changeset/962), [commit 1012](http://wiki.powerdns.com/projects/trac/changeset/1012) and others.
-   5 new regression tests have been added to insure old bugs do not return.
-   Fix small issues with very modern compilers and BOOST snapshots. Noted by Marcus Rueckert, addressed in [commit 954](http://wiki.powerdns.com/projects/trac/changeset/954), [commit 964](http://wiki.powerdns.com/projects/trac/changeset/964) [commit 965](http://wiki.powerdns.com/projects/trac/changeset/965), [commit 1003](http://wiki.powerdns.com/projects/trac/changeset/1003).

# Recursor version 3.1.4
Released the 13th of November 2006.

This release contains almost no new features, but consists mostly of minor and major bug fixes. It also addresses two major security issues, which makes this release a highly recommended upgrade.

## Security issues
-   Large TCP questions followed by garbage could cause the recursor to crash. This critical security issue has been assigned CVE-2006-4251, and is fixed in [commit 915](http://wiki.powerdns.com/projects/trac/changeset/915). More information can be found in [Section 5, “PowerDNS Security Advisory 2006-01: Malformed TCP queries can lead to a buffer overflow which might be exploitable”](security/powerdns-advisory-2006-01.md "5. PowerDNS Security Advisory 2006-01: Malformed TCP queries can lead to a buffer overflow which might be exploitable").
-   CNAME loops with zero second TTLs could cause crashes in some conditions. These loops could be constructed by malicious parties, making this issue a potential denial of service attack. This security issue has been assigned CVE-2006-4252 and is fixed by [commit 919](http://wiki.powerdns.com/projects/trac/changeset/919). More information can be found in [Section 6, “PowerDNS Security Advisory 2006-02: Zero second CNAME TTLs can make PowerDNS exhaust allocated stack space, and crash”](security/powerdns-advisory-2006-02.md "PowerDNS Security Advisory 2006-02: Zero second CNAME TTLs can make PowerDNS exhaust allocated stack space, and crash"). Many thanks to David Gavarret for helping pin down this problem.

## Bugs
-   On certain error conditions, PowerDNS would neglect to close a socket, which might therefore eventually run out. Spotted by Stefan Schmidt, fixed in commits [892](http://wiki.powerdns.com/projects/trac/changeset/892), [897](http://wiki.powerdns.com/projects/trac/changeset/897), [899](http://wiki.powerdns.com/projects/trac/changeset/899).
-   Some nameservers (including PowerDNS in rare circumstances) emit a SOA record in the authority section. The recursor mistakenly interpreted this as an authoritative "NXRRSET". Spotted by Bryan Seitz, fixed in [commit 893](http://wiki.powerdns.com/projects/trac/changeset/893).
-   In some circumstances, PowerDNS could end up with a useless (not working, or no longer working) set of nameserver records for a domain. This release contains logic to invalidate such broken NSSETs, without overloading authoritative servers. This problem had previously been spotted by Bryan Seitz, 'Cerb' and Darren Gamble. Invalidations of NSSETs can be plotted using the "nsset-invalidations" metric, available through **rec\_control get**. Implemented in [commit 896](http://wiki.powerdns.com/projects/trac/changeset/896) and [commit 901](http://wiki.powerdns.com/projects/trac/changeset/901).
-   PowerDNS could crash while dumping the cache using **rec\_control dump-cache**. Reported by Wouter of WideXS and Stefan Schmidt and many others, fixed in [commit 900](http://wiki.powerdns.com/projects/trac/changeset/900).
-   Under rare circumstances (depleted TCP buffers), PowerDNS might send out incomplete questions to remote servers. Additionally, on big-endian systems (non-Intel and non-AMD generally), sending out large TCP answers questions would not work at all, and possibly crash. Brought to our attention by David Gavarret, fixed in [commit 903](http://wiki.powerdns.com/projects/trac/changeset/903).
-   The recursor contained the potential for a dead-lock processing an invalid domain name. It is not known how this might be triggered, but it has been observed by 'Cerb' on \#powerdns. Several dead-locks where PowerDNS consumed all CPU, but did not answer questions, have been reported in the past few months. These might be fixed by [commit 904](http://wiki.powerdns.com/projects/trac/changeset/904).
-   IPv6 'allow-from' matching had problems with the least significant bits, sometimes allowing disallowed addresses, but mostly disallowing allowed addresses. Spotted by Wouter from WideXS, fixed in [commit 916](http://wiki.powerdns.com/projects/trac/changeset/916).

## Improvements
-   PowerDNS has support to drop answers from so called 'delegation only' zones. A statistic ("dlg-only-drops") is now available to plot how often this happens. Implemented in [commit 890](http://wiki.powerdns.com/projects/trac/changeset/890).
-   Hint-file parameter was mistakenly named "hints-file" in the documentation. Spotted by my Marco Davids, fixed in [commit 898](http://wiki.powerdns.com/projects/trac/changeset/898).
-   **rec\_control quit** should be near instantaneous now, as it no longer meticulously cleans up memory before exiting. Problem spotted by Darren Gamble, fixed in [commit 914](http://wiki.powerdns.com/projects/trac/changeset/914), closing [ticket 84](https://github.com/PowerDNS/pdns/issues/84).
-   init.d script no longer refers to the Recursor as the Authoritative Server. Spotted by Wouter of WideXS, fixed in [commit 913](http://wiki.powerdns.com/projects/trac/changeset/913).
-   A potentially serious warning for users of the GNU C Library version 2.5 was fixed. Spotted by Marcus Rueckert, fixed in [commit 920](http://wiki.powerdns.com/projects/trac/changeset/920).

# Recursor version 3.1.3
Released the 12th of September 2006.

Compared to 3.1.2, this release again consists of a number of mostly minor bug fixes, and some slight improvements.

Many thanks are again due to Darren Gamble who together with his team has discovered many misconfigured domains that do work with some other name servers. DNS has long been tolerant of misconfigurations, PowerDNS intends to uphold that tradition. Almost all of the domains found by Darren now work as well in PowerDNS as in other name server implementations.

Thanks to some recent migrations, this release, or something very close to it, is powering over 40 million internet connections that we know of. We appreciate hearing about successful as well as unsuccessful migrations, please feel free to notify pdns.bd@powerdns.com of your experiences, good or bad.

## Bug-fixes
-   The MThread default stack size was too small, which led to problems, mostly on 64-bit platforms. This stack size is now configurable using the **stack-size** setting should our estimate be off. Discovered by Darren Gamble, Sten Spans and a number of others. Fixed in [commit 868](http://wiki.powerdns.com/projects/trac/changeset/868).
-   Plug a small memory leak discovered by Kai and Darren Gamble, fixed in [commit 870](http://wiki.powerdns.com/projects/trac/changeset/870).
-   Switch from the excellent nedmalloc to dlmalloc, based on advice by the nedmalloc author. Nedmalloc is optimised for multithreaded operation, whereas the PowerDNS recursor is single threaded. The version of nedmalloc shipped contained a number of possible bugs, which are probably resolved by moving to dlmalloc. Some reported crashes on hitting 2G of allocated memory on 64 bit systems might be solved by this switch, which should also increase performance. See [commit 873](http://wiki.powerdns.com/projects/trac/changeset/873) for details.

## Improvements
-   The cache is now explicitly aware of the difference between authoritative and unauthoritative data, allowing it to deal with some domains that have different data in the parent zone than in the authoritative zone. Patch in [commit 867](http://wiki.powerdns.com/projects/trac/changeset/867).
-   No longer try to parse DNS updates as if they were queries. Discovered and fixed by Jan Gyselinck, fix in [commit 871](http://wiki.powerdns.com/projects/trac/changeset/871).
-   Rebalance logging priorities for less log cluttering and add IP address to a remote server error message. Noticed and fixed by Jan Gyselinck ([commit 877](http://wiki.powerdns.com/projects/trac/changeset/877)).
-   Add **logging-facility** setting, allowing syslog to send PowerDNS logging to a separate file. Added in [commit 871](http://wiki.powerdns.com/projects/trac/changeset/871).

# Recursor version 3.1.2
Released Monday 26th of June 2006.

Compared to 3.1.1, this release consists almost exclusively of bug-fixes and speedups. A quick update is recommended, as some of the bugs impact operators of authoritative zones on the internet. This version has been tested by some of the largest internet providers on the planet, and is expected to perform well for everybody.

Many thanks are due to Darren Gamble, Stefan Schmidt and Bryan Seitz who all provided excellent feedback based on their large-scale tests of the recursor.

## Bug-fixes
-   Internal authoritative server did not differentiate between 'NXDOMAIN' and 'NXRRSET', in other words, it would answer 'no such host' when an AAAA query came in for a domain that did exist, but did not have an AAAA record. This only affects users with **auth-zones** configured. Discovered by Bryan Seitz, fixed in [commit 848](http://wiki.powerdns.com/projects/trac/changeset/848).
-   ANY queries for hosts where nothing was present in the cache would not work. This did not cause real problems as ANY queries are not reliable (by design) for anything other than debugging, but did slow down the nameserver and cause unnecessary load on remote nameservers. Fixed in [commit 854](http://wiki.powerdns.com/projects/trac/changeset/854).
-   When exceeding the configured maximum amount of TCP sessions, TCP support would break and the nameserver would waste CPU trying to accept TCP connections on UDP ports. Noted by Bryan Seitz, fixed in [commit 849](http://wiki.powerdns.com/projects/trac/changeset/849).
-   DNS queries come in two flavours: recursion desired and non-recursion desired. The latter is not very useful for a recursor, but is sometimes (erroneously) used by monitoring software or load balancers to detect nameserver availability. A non-rd query would not only not recurse, but also not query authoritative zones, which is confusing. Fixed in [commit 847](http://wiki.powerdns.com/projects/trac/changeset/847).
-   Non-standard DNS TCP queries, that did occur however, could drive the recursor to 100% CPU usage for extended periods of time. This did not disrupt service immediately, but does waste a lot of CPU, possibly exhausting resources. Discovered by Bryan Seitz, fixed in [commit 858](http://wiki.powerdns.com/projects/trac/changeset/858), which is post-3.1.2-rc1.
-   The PowerDNS recursor did not honour the rare but standardised 'ANY' query class (normally 'ANY' refers to the query type, not class), upsetting the Wildfire Jabber server. Discovered and debugged by Daniel Nauck, fixed in [commit 859](http://wiki.powerdns.com/projects/trac/changeset/859), which is post-3.1.2-rc1.
-   Everybody's favorite, when starting up under high load, a bogus line of statistics was sometimes logged. Fixed in [commit 851](http://wiki.powerdns.com/projects/trac/changeset/851).
-   Remove some spurious debugging output on dropping a packet by an unauthorized host. Discovered by Kai. Fixed in [commit 854](http://wiki.powerdns.com/projects/trac/changeset/854).

## Improvements
-   Misconfigured domains, with a broken nameserver in the parent zone, should now work better. Changes motivated and suggested by Darren Gamble. This makes PowerDNS more compliant with RFC 2181 by making it prefer authoritative data over non-authoritative data. Implemented in [commit 856](http://wiki.powerdns.com/projects/trac/changeset/856).
-   PowerDNS can now listen on multiple ports, using the **local-address** setting. Added in [commit 845](http://wiki.powerdns.com/projects/trac/changeset/845).
-   A number of speedups which should have a noticeable impact, implemented in commits [850](http://wiki.powerdns.com/projects/trac/changeset/850), [852](http://wiki.powerdns.com/projects/trac/changeset/852), [853](http://wiki.powerdns.com/projects/trac/changeset/853), [855](http://wiki.powerdns.com/projects/trac/changeset/855)
-   The recursor now works around an issue with the Linux kernel 2.6.8, as shipped by Debian. Fixed by Christof Meerwald in [commit 860](http://wiki.powerdns.com/projects/trac/changeset/860), which is post 3.1.2-rc1.

# Recursor version 3.1.1
Released on the 23rd of May 2006.

**Warning**: 3.1.1 is identical to 3.1 except for a bug in the packet chaining code which would mainly manifest itself for IPv6 enabled Konqueror users with very fast connections to their PowerDNS installation. However, all 3.1 users are urged to upgrade to 3.1.1. Many thanks to Alessandro Bono for his quick aid in solving this problem.

 Many thanks are due to the operators of some of the largest internet access providers in the world, each having many millions of customers, who have tested the various 3.1 pre-releases for suitability. They have uncovered and helped fix bugs that could impact us all, but are only (quickly) noticeable with such vast amounts of DNS traffic.

After version 3.0.1 has proved to hold up very well under tremendous loads, 3.1 adds important new features

-   Ability to serve authoritative data from 'BIND' style zone files (using **auth-zones** statement).
-   Ability to forward domains so configured to external servers (using **forward-zones**).
-   Possibility of 'serving' the contents of `/etc/hosts` over DNS, which is very well suited to simple domestic router/DNS setups. Enabled using **export-etc-hosts**.
-   As recommended by recent standards documents, the PowerDNS recursor is now authoritative for RFC-1918 private IP space zones by default (suggested by Paul Vixie).
-   Full outgoing IPv6 support (off by default) with IPv6 servers getting equal treatment with IPv4, nameserver addresses are chosen based on average response speed, irrespective of protocol.
-   Initial Windows support, including running as a service ('NET START "POWERDNS RECURSOR"'). **rec\_channel** is still missing, the rest should work. Performance appears to be below that of the UNIX versions, this situation is expected to improve.

## Bug fixes
-   No longer send out SRV and MX record priorities as zero on big-endian platforms (UltraSPARC). Discovered by Eric Sproul, fixed in [commit 773](http://wiki.powerdns.com/projects/trac/changeset/773).
-   SRV records need additional processing, especially in an Active Directory setting. Reported by Kenneth Marshall, fixed in [commit 774](http://wiki.powerdns.com/projects/trac/changeset/774).
-   The root-records were not being refreshed, which could lead to problems under inconceivable conditions. Fixed in [commit 780](http://wiki.powerdns.com/projects/trac/changeset/780).
-   Fix resolving domain names for nameservers with multiple IP addresses, with one of these addresses being lame. Other nameserver implementations were also unable to resolve these domains, so not a big bug. Fixed in [commit 780](http://wiki.powerdns.com/projects/trac/changeset/780).
-   For a period of 5 minutes after expiring a negative cache entry, the domain would not be re-cached negatively, leading to a lot of duplicate outgoing queries for this short period. This fix has raised the average cache hit rate of the recursor by a few percent. Fixed in [commit 783](http://wiki.powerdns.com/projects/trac/changeset/783).
-   Query throttling was not aggressive enough and not all sorts of queries were throttled. Implemented in [commit 786](http://wiki.powerdns.com/projects/trac/changeset/786).
-   Fix possible crash during startup when parsing empty configuration lines ([commit 807](http://wiki.powerdns.com/projects/trac/changeset/807)).
-   Fix possible crash when the first query after wiping a cache entry was for the just deleted entry. Rare in production servers. Fixed in [commit 820](http://wiki.powerdns.com/projects/trac/changeset/820).
-   Recursor would send out differing TTLs when receiving a misconfigured, standards violating, RRSET with different TTLs. Implement fix as mandated by RFC 2181, paragraph 5.2. Reported by Stephen Harker ([commit 819](http://wiki.powerdns.com/projects/trac/changeset/819)).
-   The **top-remotes** would list remotes more than once, once per source port. Discovered by Jorn Ekkelenkamp, fixed in [commit 827](http://wiki.powerdns.com/projects/trac/changeset/827), which is post 3.1-pre1.
-   Default **allow-from** allowed queries from fe80::/16, corrected to fe80::/10. Spotted by Niels Bakker, fixed in [commit 829](http://wiki.powerdns.com/projects/trac/changeset/829), which is post 3.1-pre1.
-   While PowerDNS blocks failing queries quickly, multiple packets could briefly be in flight for the same domain and nameserver. This situation is now explicitly detected and queries are chained to identical queries already in flight. Fixed in [commit 833](http://wiki.powerdns.com/projects/trac/changeset/833) and [commit 834](http://wiki.powerdns.com/projects/trac/changeset/834), post 3.1-pre1.

## Improvements
-   ANY queries are now implemented as in other nameserver implementations, leading to a decrease in outgoing queries. The RFCs are not very clear on desired behaviour, what is implemented now saves bandwidth and CPU and brings us in line with existing practice. Previously ANY queries were not cached by the PowerDNS recursor. Implemented in [commit 784](http://wiki.powerdns.com/projects/trac/changeset/784).
-   **rec\_control** was very sparse in its error reporting, and user unfriendly as well. Reported by Erik Bos, fixed in [commit 818](http://wiki.powerdns.com/projects/trac/changeset/818) and [commit 820](http://wiki.powerdns.com/projects/trac/changeset/820).
-   IPv6 addresses were printed in a non-standard way, fixed in [commit 788](http://wiki.powerdns.com/projects/trac/changeset/788).
-   TTLs of records are now capped at two weeks, [commit 820](http://wiki.powerdns.com/projects/trac/changeset/820).
-   **allow-from** IPv4 netmasks now automatically work for IP4-to-IPv6 mapper IPv4 addresses, which appear when running on the wildcard **::** IPv6 address. Lack of feature noted by Marcus 'darix' Rueckert. Fixed in [commit 826](http://wiki.powerdns.com/projects/trac/changeset/826), which is post 3.1-pre1.
-   Errors before daemonizing are now also sent to syslog. Suggested by Marcus 'darix' Rueckert. Fixed in [commit 825](http://wiki.powerdns.com/projects/trac/changeset/825), which is post 3.1-pre1.
-   When launching without any form of configured network connectivity, all root-servers would be cached as 'down' for some time. Detect this special case and treat it as a resource-constraint, which is not accounted against specific nameservers. Spotted by Seth Arnold, fixed in [commit 835](http://wiki.powerdns.com/projects/trac/changeset/835), which is post 3.1-pre1.
-   The recursor now does not allow authoritative servers to keep supplying its own NS records into perpetuity, which causes problems when a domain is redelegated but the old authoritative servers are not updated to this effect. Noticed and explained at length by Darren Gamble of Shaw Communications, addressed by [commit 837](http://wiki.powerdns.com/projects/trac/changeset/837), which is post 3.1-pre2.
-   Some operators may want to follow RFC 2181 paragraph 5.2 and 5.4. This harms performance and does not solve any real problem, but does make PowerDNS more compliant. If you want this, enable **auth-can-lower-ttl**. Implemented in [commit 838](http://wiki.powerdns.com/projects/trac/changeset/838), which is post 3.1-pre2.

# Recursor version 3.0.1
Released 25th of April 2006, [download](http://www.powerdns.com/en/downloads.aspx).

This release consists of nothing but tiny fixes to 3.0, including one with security implications. An upgrade is highly recommended.

-   Compilation used both `cc` and `gcc`, leading to the possibility of compiling with different compiler versions ([commit 766](http://wiki.powerdns.com/projects/trac/changeset/766)).
-   **rec\_control** would leave files named `lsockXXXXXX` around in the configured socket-dir. Operators may wish to remove these files from their socket-dir (often `/var/run`), quite a few might have accumulated already ([commit 767](http://wiki.powerdns.com/projects/trac/changeset/767)).
-   Certain malformed packets could crash the recursor. As far as we can determine these packets could only lead to a crash, but as always, there are no guarantees. A quick upgrade is highly recommended (commits [760](http://wiki.powerdns.com/projects/trac/changeset/760), [761](http://wiki.powerdns.com/projects/trac/changeset/761)). Reported by David Gavarret.
-   Recursor would not distinguish between NXDOMAIN and NXRRSET ([commit 756](http://wiki.powerdns.com/projects/trac/changeset/756)). Reported and debugged by Jorn Ekkelenkamp.
-   Some error messages and trace logging statements were improved (commits [756](http://wiki.powerdns.com/projects/trac/changeset/756), [758](http://wiki.powerdns.com/projects/trac/changeset/758), [759](http://wiki.powerdns.com/projects/trac/changeset/759)).
-   stderr was closed during daemonizing, but not dupped to /dev/null, leading to slight chance of odd behaviour on reporting errors ([commit 757](http://wiki.powerdns.com/projects/trac/changeset/757))

## Operating system specific fixes
-   The stock Debian sarge Linux kernel, 2.6.8, claims to support epoll but fails at runtime. The epoll self-testing code has been improved, and PowerDNS will fall back to a select based multiplexer if needed ([commit 758](http://wiki.powerdns.com/projects/trac/changeset/758)) Reported by Michiel van Es.
-   Solaris 8 compilation and runtime issues were addressed. See the README for details ([commit 765](http://wiki.powerdns.com/projects/trac/changeset/765)). Reported by Juergen Georgi and Kenneth Marshall.
-   Solaris 10 x86\_64 compilation issues were addressed ([commit 755](http://wiki.powerdns.com/projects/trac/changeset/755)). Reported and debugged by Eric Sproul.

# Recursor version 3.0
Released 20th of April 2006, [download](http://www.powerdns.com/en/downloads.aspx).

This is the first separate release of the PowerDNS Recursor. There are many reasons for this, one of the most important ones is that previously we could only do a release when both the recursor and the authoritative nameserver were fully tested and in good shape. The split allows us to release new versions when each part is ready.

Now for the real news. This version of the PowerDNS recursor powers the network access of over two million internet connections. Two large access providers have been running pre-releases of 3.0 for the past few weeks and results are good. Furthermore, the various pre-releases have been tested nearly non-stop with DNS traffic replayed at 3000 queries/second.

As expected, the 2 million households shook out some very rare bugs. But even a rare bug happens once in a while when there are this many users.

We consider this version of the PowerDNS recursor to be the most advanced resolver publicly available. Given current levels of spam, phishing and other forms of internet crime we think no recursor should offer less than the best in spoofing protection. We urge all operators of resolvers without proper spoofing countermeasures to consider PowerDNS, as it is a Better Internet Nameserver Daemon.

A good article on DNS spoofing can be found [here](http://www.securesphere.net/download/papers/dnsspoof.htm). Some more information, based on a previous version of PowerDNS, can be found on the [PowerDNS development blog](http://blog.netherlabs.nl/articles/2006/04/14/holy-cow-1-3-million-additional-ip-addresses-served-by-powerdns).

**Warning**: Because of recent DNS based denial of service attacks, running an open recursor has become a security risk. Therefore, unless configured otherwise this version of PowerDNS will only listen on localhost, which means it does not resolve for hosts on your network. To fix, configure the **local-address** setting with all addresses you want to listen on. Additionally, by default service is restricted to RFC 1918 private IP addresses. Use **allow-from** to selectively open up the recursor for your own network. See [pdns\_recursor settings](recursor/settings.md#allow-from "pdns_recursor settings") for details.

## Important new features of the PowerDNS recursor 3.0
-   Best spoofing protection and detection we know of. Not only is spoofing made harder by using a new network address for each query, PowerDNS detects when an attempt is made to spoof it, and temporarily ignores the data. For details, see [Anti-spoofing](recursor/security.md "Anti-spoofing").
-   First nameserver to benefit from epoll/kqueue/Solaris completion ports event reporting framework, for stellar performance.
-   Best statistics of any recursing nameserver we know of, see [Statistics](recursor/stats.md "Statistics").
-   Last-recently-used based cache cleanup algorithm, keeping the 'best' records in memory
-   First class Solaris support, built on a 'try and buy' Sun CoolThreads T 2000.
-   Full IPv6 support, implemented natively.
-   Access filtering, both for IPv4 and IPv6.
-   Experimental SMP support for nearly double performance. See [PowerDNS Recursor performance](recursor/performance.md "PowerDNS Recursor performance").

Many people helped package and test this release. Jorn Ekkelenkamp of ISP-Services helped find the '8000 SOAs' bug and spotted many other oddities and [XS4ALL](http://www.xs4all.nl) internet funded a lot of the recent development. Joaquín M López Muñoz of the boost::multi\_index\_container was again of great help.

# Version 2.9.20
Released the 15th of March 2006

Besides adding OpenDBX, this release is mostly about fixing problems and speeding up the recursor. This release has been made possible by [XS4ALL](http://www.xs4all.nl) and [True](http://true.nl). Thanks!

Furthermore, we are very grateful for the help of Andrew Pinski, who hacks on gcc, and of Joaquín M López Muñoz, the author of [boost::multi\_index\_container](http://www.boost.org/libs/multi_index/doc/index.html). Without their near-realtime help this release would've been delayed a lot. Thanks!

## Bugs fixed in the recursor
-   Possible stability issues in the recursor on encountering errors ([commit 532](http://wiki.powerdns.com/projects/trac/changeset/532), [commit 533](http://wiki.powerdns.com/projects/trac/changeset/533))
-   Memory leaks in recursor fixed ([commit 534](http://wiki.powerdns.com/projects/trac/changeset/534), [commit 572](http://wiki.powerdns.com/projects/trac/changeset/572)). In a test 800 million real life DNS packets have been sent to the recursor, representing several days of traffic from a major ISP, memory use was high (500MB), but stable.
-   Prune all data in PowerDNS - previously per-nameserver and per-query performance statistics were kept around forever ([commit 535](http://wiki.powerdns.com/projects/trac/changeset/535))
-   IPv6 additional processing was broken. Reported by Lionel Elie Mamane, who also provided a fix. The problem was fixed differently in the end. [commit 562](http://wiki.powerdns.com/projects/trac/changeset/562).
-   pdns\_recursor did not shuffle answers since 2.9.19, leading to problems sending mail to the Hotmail servers. Reported in [ticket 54](https://github.com/PowerDNS/pdns/issues/54), fixed in [commit 567](http://wiki.powerdns.com/projects/trac/changeset/567).
-   If a single nameserver had multiple IP addresses listed, PowerDNS would only use one of them. Noted by Mark Martin, fixed in [commit 570](http://wiki.powerdns.com/projects/trac/changeset/570), who depends on a domain with 4 nameserver IP addresses of which 2 are broken.

## Improvements to the recursor
-   Commits [535](http://wiki.powerdns.com/projects/trac/changeset/535), [540](http://wiki.powerdns.com/projects/trac/changeset/540), [541](http://wiki.powerdns.com/projects/trac/changeset/541), [542](http://wiki.powerdns.com/projects/trac/changeset/542), [543](http://wiki.powerdns.com/projects/trac/changeset/543), [544](http://wiki.powerdns.com/projects/trac/changeset/544), [545](http://wiki.powerdns.com/projects/trac/changeset/545), [547](http://wiki.powerdns.com/projects/trac/changeset/547) and [548](http://wiki.powerdns.com/projects/trac/changeset/548), [574](http://wiki.powerdns.com/projects/trac/changeset/574) all speed up the recursor by a large factor, without altering the DNS algorithm.
-   Move recursor to the incredible boost::multi\_index\_container ([commit 580](http://wiki.powerdns.com/projects/trac/changeset/580)). This brings a huge improvement in cache pruning times.
-   [commit 549](http://wiki.powerdns.com/projects/trac/changeset/549) and [commit 550](http://wiki.powerdns.com/projects/trac/changeset/550) work around gcc bug [24704](http://gcc.gnu.org/bugzilla/show_bug.cgi?id=24704) if requested, which speeds up the recursor a lot, but involves a dirty hack. Enable with **./configure --enable-gcc-skip-locking**. No guarantees!

## Bugs fixed in the authoritative nameserver
-   PowerDNS would no longer allow a '/' in domain names, fixed by [commit 537](http://wiki.powerdns.com/projects/trac/changeset/537), reported in [ticket 48](https://github.com/PowerDNS/pdns/issues/48).
-   Parameters to **pdns\_control notify-host** were not checked, leading to possible crashes. Reported in [ticket 24](https://github.com/PowerDNS/pdns/issues/24), fixed in [commit 565](http://wiki.powerdns.com/projects/trac/changeset/565).
-   On some compilers, processing of NAPTR records could cause the server to crash. Reported by Bernd Froemel in [ticket 29](https://github.com/PowerDNS/pdns/issues/29), fixed in [commit 538](http://wiki.powerdns.com/projects/trac/changeset/538).
-   Backend errors could make the whole nameserver exit under some circumstances, notably using the LDAP backend. Fixed in [commit 583](http://wiki.powerdns.com/projects/trac/changeset/583), reported in [ticket 62](https://github.com/PowerDNS/pdns/issues/62).
-   Referrals were subtly broken by recent CNAME/Wildcard improvements, fixed in [commit 539](http://wiki.powerdns.com/projects/trac/changeset/539). Fix and other improvements sponsored by [True](http://true.nl).
-   PowerDNS would try to insert records it has no knowledge about in slave zones, which did not work. Reported in [ticket 60](https://github.com/PowerDNS/pdns/issues/60), fixed in [commit 566](http://wiki.powerdns.com/projects/trac/changeset/566). A superior fix would be to implement the relevant unknown record standard.

## Improvements to the authoritative nameserver
-   Pipebackend did not properly propagate the ABI version to its children, fixed in [commit 546](http://wiki.powerdns.com/projects/trac/changeset/546), reported by kickdaddy@gmail.com in [ticket 45](https://github.com/PowerDNS/pdns/issues/45).
-   [OpenDBX](http://www.linuxnetworks.de/pdnsodbx/index.html) backend added ([commit 559](http://wiki.powerdns.com/projects/trac/changeset/559), [commit 560](http://wiki.powerdns.com/projects/trac/changeset/560), [commit 561](http://wiki.powerdns.com/projects/trac/changeset/561)) by Norbert Sendetzky. From the website: “ The OpenDBX backend enables it to fetch DNS information from every DBMS supported by the OpenDBX library and combines the power of one of the best DNS server implementations with the flexibility of the OpenDBX library. ” OpenDBX adds some other features like database failover. Thanks Norbert!
-   LDAP fixes as reported in [ticket 37](https://github.com/PowerDNS/pdns/issues/37), fixed in [commit 558](http://wiki.powerdns.com/projects/trac/changeset/558), which make **pdns\_control notify** work.
-   Arjo Hooimeijer added support for soa-refresh-default, soa-retry-default, soa-expire-default, which were previously hardcoded. [commit 563](http://wiki.powerdns.com/projects/trac/changeset/563) and fallout in [commit 573](http://wiki.powerdns.com/projects/trac/changeset/573) (thanks to Wolfram Schlich).

## Miscellaneous
-   Fixes for g++ 4.1. Compiling with 4.1 realizes notable speedups. [commit 568](http://wiki.powerdns.com/projects/trac/changeset/568), [commit 569](http://wiki.powerdns.com/projects/trac/changeset/569).
-   PowerDNS now reports if it is running in 32 or 64 bit mode, useful for bi-arch users that need to know if they are benefitting from [AMD's great processor](http://www.amd.com). [commit 571](http://wiki.powerdns.com/projects/trac/changeset/571).
-   **dnsscope** compiles again, [commit 551](http://wiki.powerdns.com/projects/trac/changeset/551), [commit 564](http://wiki.powerdns.com/projects/trac/changeset/564) (FreeBSD 64-bit time\_t).
-   **dnsreplay\_mindex** compiles again, fixed by [commit 572](http://wiki.powerdns.com/projects/trac/changeset/572). Its performance, and the performance of the recursor was improved by [commit 559](http://wiki.powerdns.com/projects/trac/changeset/559).
-   Build scripts were added, mostly for internal use but we know some PowerDNS users build their own packages too. [commit 553](http://wiki.powerdns.com/projects/trac/changeset/553), [commit 554](http://wiki.powerdns.com/projects/trac/changeset/554), [commit 555](http://wiki.powerdns.com/projects/trac/changeset/555), [commit 556](http://wiki.powerdns.com/projects/trac/changeset/556), [commit 557](http://wiki.powerdns.com/projects/trac/changeset/557).
-   `bootstrap` script was not included in release. Thanks to Stefan Arentz for noticing. Fixed in [commit 574](http://wiki.powerdns.com/projects/trac/changeset/574).

# Version 2.9.19
Released 29th of October 2005.

As with other recent releases, the usage of PowerDNS appears to have skyrocketed. Informal, though strict, measurements show that PowerDNS now powers around 50% of all German domains, and somewhere in the order of 10-15% of the rest of the world. Furthermore, DNS is set to take a central role in connecting Voice over IP providers, with PowerDNS offering a very good feature set for these ENUM deployments. PowerDNS is already powering the E164.info ENUM zone and also acts as the backend for a major VoIP provisioning platform.

Included in this release is the now complete packet parsing/generating, record parsing/generating infrastructure. Furthermore, this framework is used by the recursor, hopefully making it very fast, memory efficient and robust. Many records are now processed using a single line of code. This has made the recursor a lot stricter in packet parsing, you will see some error messages which did not appear before. Rest assured however that these only happen for queries which have no valid answer in any case.

Furthermore, support for DNSSEC records is available in the new infrastructure, although is should be emphasised that there is more to DNSSEC than parsing records. There is no real support for DNSSEC (yet).

Additionally, the BIND Backend has been replaced by what was up to now known as the 'Bind2Backend'. Initial benchmarking appears to show that this backend is faster, uses less memory and has shorter startup times. The code is also shorter.

This release fixes a number of embarrassing bugs and is a recommended upgrade.

Thanks are due to [XS4ALL](http://www.xs4all.nl) who are supporting continuing development of PowerDNS, the fruits of which can be found in this release already. Furthermore, a remarkable number of people have helped report bugs, validate solutions or have submitted entire patches. Many thanks!

## Improvements
-   dnsreplay now has a help message and has received further massive updates, making the code substantially faster. It turns out that dnsreplay is often 'heavier' than the PowerDNS process being benchmarked.
-   PowerDNS recursor no longer prints out its queries by default as most recursor deployments have too much traffic for this to be useful.
-   PowerDNS recursor is now able to read its root-hints from disk, which is useful to operate with alternate roots, like the [Open Root Server Network](http://www.orsn.org). See [PowerDNS Recursor](recursor/index.md).
-   PowerDNS can now send out old-fashioned root-referrals when queried for domains for which it is not authoritative. Wastes some bandwidth but may solve incoming query floods if domains are delegated to you for which you are not authoritative, but which are queried by broken recursors.
-   PowerDNS now prints out a warning when running with legacy LinuxThreads implementation instead of the high performance NPTL library. [commit 455](http://wiki.powerdns.com/projects/trac/changeset/455).
-   A lot of superfluous calls to gettimeofday() have been removed, making PowerDNS and especially the recursor faster. Suggested by Kai.
-   SPF records are now supported natively. [commit 472](http://wiki.powerdns.com/projects/trac/changeset/472), closing [ticket 22](https://github.com/PowerDNS/pdns/issues/22).
-   Improved IPv6 'bound to' messages. Thanks to Niels Bakker, Wichert Akkerman and Gerty de Wolf for suggestions.
-   Separate graphs can now be made of IPv6 queries and answers. [commit 485](http://wiki.powerdns.com/projects/trac/changeset/485).
-   Out of zone additional processing is now on by default to better comply with standards. [commit 487](http://wiki.powerdns.com/projects/trac/changeset/487).
-   Regression tests have been expanded to deal with more record types (SRV, NAPTR, TXT, duplicate SRV).
-   Improved query-logging in Bindbackend, which can be used for debugging purposes.
-   Dropped libpcap dependency, making compilation easier
-   pdns\_control now has a help message.
-   Add RRSIG, DNSKEY, DS and NSEC records for DNSSEC-bis to new parser infrastructure.
-   Recursor now honours EDNS0 allowing it to send out larger answers.

## Bugs fixed
-   Domain name validation has been made a lot stricter - it turns out PostgreSQL was interpreting some (corrupt) domain names as unicode. Tested and suggested by Register.com ([commit 451](http://wiki.powerdns.com/projects/trac/changeset/451)).
-   LDAP backend did not compile (commits [452](http://wiki.powerdns.com/projects/trac/changeset/452), [453](http://wiki.powerdns.com/projects/trac/changeset/453)) due to partially applied patch (Norbert Sendetzky)
-   Incoming zone transfers work reliably again. Fixed in [commit 460](http://wiki.powerdns.com/projects/trac/changeset/460) and beyond. And [commit 523](http://wiki.powerdns.com/projects/trac/changeset/523) - closing Debian bug 330184.
-   Recent g++ versions exposed a mistake in the PowerDNS recursor cache pruning code, causing random crashes. Fixed in [commit 465](http://wiki.powerdns.com/projects/trac/changeset/465). Reported by several Red Hat users.
-   PowerDNS recursor, and MTasker in general, did not work on Solaris. Patch by Juergen Ilse, [commit 471](http://wiki.powerdns.com/projects/trac/changeset/471). Also moved most of PowerDNS over to uint32\_t style typedefs, which eases compilation problems on Solaris, [commit 477](http://wiki.powerdns.com/projects/trac/changeset/477).
-   Bindbackend2 did not properly search its include path for $INCLUDE statements. Noted by Mark Bergsma, [commit 474](http://wiki.powerdns.com/projects/trac/changeset/474).
-   Bindbackend did not notice changed zones, this problem has been fixed by the move to Bind2.
-   Pipebackend did not clean up, leading to an additional pipe backend per AXFR or pdns\_control reload. Discovered by Marc Jauvin, fixed by [commit 525](http://wiki.powerdns.com/projects/trac/changeset/525).
-   Bindbackend (both old and current versions) did not honour 'include' statements in `named.conf` on **pdns\_control rediscover**. Noted by Marc Jauvin, fixed by [commit 526](http://wiki.powerdns.com/projects/trac/changeset/526).
-   Zone transfers were sometimes shuffled, which wastes useless time, [commit 478](http://wiki.powerdns.com/projects/trac/changeset/478).
-   CNAMEs and Wildcards now work as in Bind, fixing many complaints, [commit 487](http://wiki.powerdns.com/projects/trac/changeset/487).
-   NAPTR records were compressed, which would work, but was in violation of the RFC, commit 493.
-   NAPTR records were not always parsed correctly from BIND zone files, fixed, commit 494.
-   Geobackend needed additional include statement to compile on more recent Linux distributions, commit 496.

# Version 2.9.18
Released on the 16th of July 2005.

The '8 million domains' release, which also marks the battle readiness of the PowerDNS Recursor. The latest improvements have been made possible by financial support and contributions by [Register.com](http://register.com) and [XS4ALL](http://www.xs4all.nl/). Thanks!

This release brings a number of new features (vastly improved recursor, Generic Oracle Support, DNS analysis and replay tools, and more) but also has a new build dependency, the [Boost library](http://www.boost.org) (version 1.31 or higher).

Currently several big ISPs are evaluating the PowerDNS recursor for their resolving needs, some of them have switched already. In the course of testing, over 350 million actual queries have been recorded and replayed, the answers turn out to be satisfactorily.

This testing has verified that the pdns recursor, as shipped in this release, can stand up to heavy duty ISP loads (over 20000 queries/second) and in fact does so better than major other nameservers, giving more complete answers and being faster to boot.

We invite ISPs who note recursor problems to record their problematic traffic and replay it using the tools described in [Tools to analyse DNS traffic](tools/analysis.md "Tools to analyse DNS traffic") to discover if PowerDNS does a better job, and to let us know the results.

Additionally, the bind2backend is almost ready to replace the stock bind backend. If you run with Bind zones, you are cordially invited to substitute 'launch=bind2' for 'launch=bind'. This will happen automatically in 2.9.19!

In other news, the entire Wikipedia constellation now runs on PowerDNS using the Geo Backend! Thanks to Mark Bergsma for keeping us updated.

There are two bugs with security implications, which only apply to installations running with the LDAP backend, or installations providing recursion to a limited range of IP addresses. If any of these apply to you, an upgrade is highly advised

-   The LDAP backend did not properly escape all queries, allowing it to fail and not answer questions. We have not investigated further risks involved, but we advise LDAP users to update as quickly as possible (Norbert Sendetzky, Jan de Groot)
-   Questions from clients denied recursion could blank out answers to clients who are allowed recursion services, temporarily. Reported by Wilco Baan. This would've made it possible for outsiders to blank out a domain temporarily to your users. Luckily PowerDNS would send out SERVFAIL or Refused, and not a denial of a domain's existence.

## General bugs fixed

-   TCP authoritative server would not relaunch a backend after failure (reported by Norbert Sendetzky)
-   Fix backend restarting logic (reported, and fix suggested by Norbert Sendetzky)
-   Launching identical backends multiple times, with different settings, did not work. Reported by Mario Manno.
-   Master/slave queries did not honour the **query-local-address** setting. Spotted by David Levy of Register.com. The fix also randomises the local port used, slightly improving security.

## Compilation fixes
-   Fix compile on Solaris, they define 'PC' for some reason. Reported by Eric Yiu.
-   PowerDNS recursor would not compile on FreeBSD due to Linux specific defines, as reported in cvstrac ticket 26 (Ralf van der Enden)
-   Several 64 bits issues have been fixed, especially in the Logging subsystem.
-   SSQLite would fail to compile on recent Debian systems (Matthijs Möhlmann)
-   Generic MySQL would not compile on 64-bit platforms.

## Improvements
-   PowerDNS now reports stray command line arguments, like when running '--local-port 5300' instead of '--local-port=5300'. Reported by Christian Welzel.
-   We now warn against erroneous logging-facility specification, ie specifying an unknown facility.
-   **--version** now outputs gcc version used, so we can tell people 2.95 is no longer supported.
-   Extended regression tests, moved them to the new 'sdig' tool (see below).
-   Bind2backend is now blazingly fast, and highly memory efficient to boot. As a special bonus it can read gzipped zones directly. The '.NET' zone is hosted using 401MB of memory, the same size as the zone on disk.
-   The Pipe Backend has been improved such that it can send out different answers based on the IP address the question was received ON. See [PipeBackend protocol](authoritative/backend-pipe.md#pipebackend-protocol) for how this changed the Pipe Backend protocol. Note that you need to set **pipebackend-abi-version** to benefit from this change, existing clients are not affected. Change and documentation contributed by Marc Jauvin of Register4Less.
-   LDAP backend has been updated (Norbert Sendetzky).

## Recursor improvements and fixes.
See [Recursion](authoritative/recursion.md "Recursion") for details. The changes below mean that all of the caveats listed for the recursor have now been addressed.

-   After half an hour of uptime, the entire cache would be pruned for each packet, which is a tad slow. It now appears the pdns recursor is among the fastest around.
-   Under high loads, or when unlucky, some query mthreads would get 'stuck', and show up in the statistics as eternally running queries.
-   Lots of redundant gettimeofday() and time() calls were removed, which has resulted in a measurable speedup.
-   pdns\_recursor can now listen on several addresses simultaneously.
-   Now supports setuid and setgid operation to allow running as a less privileged user (Bram Vandoren).
-   Return code of pdns\_recursor binary did not make sense (Matthijs Möhlmann and Thomas Hood)
-   Timeouts and errors are now split out in statistics.
-   Many people reported broken statistics, it turned out that no statistics were being reported if there had been no questions to base them on. We now log a message to that effect.
-   Add **query-local-address** support, which allows the recursor to send questions from a specific IP address. Useful for anycast setups.
-   Add outgoing TCP query support and proper truncated answer support. Needed for Worldnic Denial of Service protection, which sends out truncated packets to force clients to connect over TCP, which prevents spoofing.
-   Properly truncate our own answers.
-   Improve our TCP answers by using writev, which is slightly friendlier to the network.
-   On FreeBSD, TCP errors could cause the recursor to exit suddenly due to a SIGPIPE signal.
-   Maximum number of simultaneous client TCP connections can now be limited with the **max-tcp-clients** setting.
-   Add aggressive timeouts for TCP clients to make sure resources are not wasted. Defaults to two seconds, can be configured with the **client-tcp-timeout** setting.

## Backend fixes
-   SQLite backend would not slave properly (Darron Broad)
-   Generic MySQL would not compile on 64-bit platforms.

## New technology
-   Added the new DNS parser logic, called MOADNSParser. Completely modular, every memory access checked.
-   'sdig', a simple dig work-alike with 'canonical' output, which is used for the regression tests. Based on the new DNS parser logic.
-   **dnswasher**, **dnsreplay** and **dnsscope**, all DNS analysis tools. See [Tools to analyse DNS traffic](tools/analysis.md "Tools to analyse DNS traffic") for more details.
-   Generic Oracle Backend, sponsored by Register.COM. See [Oracle specifics](authoritative/backend-generic-oracle.md "Oracle specifics").

# Version 2.9.17

See [the new timeline](http://wiki.powerdns.com/trac/timeline) for progress reports.

The 'million domains' release - PowerDNS has now firmly established itself as a major player with the unofficial count (ie, guesswork) now at over two million PowerDNS domains! Also, the GeoBackend has been tested by a big website and may soon see wider deployment. Thanks to Mark Bergsma for spreading the word!

It is also a release with lots of changes and fixes. Take care when deploying!

## Security issues
-   PowerDNS could be temporarily DoSed using a random stream of bytes. Reported cause of this has been fixed.

## Enhancements
-   Reported version can be changed, or removed - see the "version-string" setting.
-   Duplicate MX records are now no longer considered duplicate if their priorities differ. Some people need this feature for spam filtering.

## Bug fixes
-   NAPTR records can now be slaved, patch by Lorens Kockum.
-   GMySQL now works on Solaris
-   PowerDNS could be confused by questions with a %-sign in them - fixing cvstrac ticket \#16 (reported by dilinger at voxel.net)
-   An authentication bug in the webserver was possibly fixed, please report if you were suffering from this. Being unable to authenticate to the webserver was what you would've noticed.
-   Fix for cvstrac ticket \#2, PowerDNS could lose sync when sending out a very large number of notifications. Excellent bug report by Martin Hoffman, who also improved our original bugfix.
-   Fix the oldest PowerDNS bug in existence - under some circumstances, PowerDNS would log to syslog one character at a time. This was cvstrac ticket \#4
-   HINFO records can now be slaved, fixing cvstrac ticket \#8.
-   pdns\_recursor could block under some circumstances, especially in case of corrupt UDP packets. Reported by Wichert Akkerman. Fix by Christopher Meer. This was cvstrac ticket \#13.
-   Large SOA serial numbers would sometimes be logged as a signed integer, leading to negative numbers in the log.
-   PowerDNS now fully supports 32 bit SOA serial numbers (thanks to Mark Bergsma), closing cvstrac ticket \#5.
-   pdns\_recursor --local-address help text was wrong.
-   Very devious bug - PowerDNS did not clear its cache before sending out update notifications, leading slaves to conclude there was no update to AXFR. Excellent debugging by mkuchar at wproduction.cz.
-   Probably fixed cvstrac ticket \#26, which caused pdns\_recursor to fail on recent FreeBSD 5.3 systems. Please check, I have no such system to test on.
-   Geobackend did not get built for Debian.

# Version 2.9.16
The 'it must still be Friday somewhere' release. Massive number of fixes, portability improvements and the new Geobackend by Mark Bergsma & friends.

## New
-   The Geobackend which makes it possible to send different answers to different IP ranges. Initial documentation can be found in pdns/modules/geobackend/README.
-   qgen query generation tool. Nearly completely undocumented and hard to build too, it requires Boost. But very spiffy. Use **cd pdns; make qgen** to build it.

## Bugfixes
-   The most reported bug ever was fixed. Zone2sql required the inclusion of unistd.h, except on Debian unstable.
-   PowerDNS tried to listen on its control "pipe" which does not work. Probably harmless, but might have caused some oddities.
-   The Packet Cache did not always set its TTL immediately, causing some packets to be inserted, even when running with the cache disabled (Mark Bergsma).
-   Valgrind found some uninitialized reads, causing bogus values in the priority field when it was not needed.
-   Valgrind found a bug in MTasker where we used delete instead of delete[].
-   SOA serials and other parameters are unsigned. This means that very large SOA serial numbers would be messed up (Michel Stol, Stefano Straus)
-   PowerDNS left its controlsocket around after exit and reported confusing errors if a socket was already in use.
-   The recursor proxy did not work on big endian systems like SPARC and some MIPS processors (Remco Post)
-   We no longer dump core on processing LOC records on UltraSPARC (Andrew Mulholland supplied a testing machine)

## Improvements
-   MySQL can now connect to a specified port again (Chris Anderton).
-   When running chroot()ed and with master or slave support active, PowerDNS needs to resolve domain names to find slaves. This in turn may require access to certain libraries. Previously, these needed to be available in the chroot directory but by forcing an initial lookup, these libraries are now loaded before the chrooting.
-   pdns\_recursor was very slow after having done a larger number of queries because of the checks to see if a query should be throttled. This is now done using a set which is a lot faster than the previous full sequential scan.
-   The throttling code may not have throttled as much as was configured.
-   Yet another big LDAP update. The LDAP backend now load balances connections over several hosts (Norbert Sendetzky)
-   Updated b.root-servers.net address in the recursor

# Version 2.9.15
This release fixes up some of the shortcomings in 2.9.14, and adds some new features too.

## Bugfixes
-   **allow-recursion-override** was on by default, it was meant to be off.
-   Logging was still off in daemon mode, fixed.
-   debian/rules forgot to build an sqlite package
-   Recursor accidentally linked in MySQL - this was the result of an experiment with a persistent recursor cache.
-   The PowerDNS recursor had stability problems. It now sorts nameservers (roughly) by responsiveness. The 'roughly' part upset the sorting algorithm used, the speeds being sorted on changed during sorting.
-   The recursor now outputs the nameserver average response times in trace mode
-   LDAP compiles again.

## Improvements
-   zone2sql can now accept `-` as a file name which causes it to read stdin. This allows the following to work: **dig axfr example.org | zone2sql --gmysql --zone=- | mysql pdns**, which is a nice way to import a zone.
-   zone2sql now ignores duplicate SOA records which are identical - which also makes the above possible.
-   Remove libpqpp dependencies - since we now use the native C API for PostgreSQL

# Version 2.9.14

Big release with the fix for the all important 2^30 seconds problem and a lot of other news.
-   errno problems would cause compilation problems when using LDAP (Norbert Sendetzky)
-   The Generic SQL backend could cause crashes on PostgreSQL when using pdns\_control notify (Georg Bauer)
-   Debian compatible init.d script (Wichert Akkerman)
-   If using the master or slave features, pdns had the notion of eternity ending in 2038, except that due to a thinko, eternity ended out to be the 10th of January 2004. This caused a loop to timeout immediately. Many thanks to Jasper Spaans for spotting the bug within five minutes.
-   Parts of the SOA field were not canonicalized.
-   The loglevel could in fact cause nothing to be logged (Norbert Sendetzky)

## Improvements
-   The recursor now chooses the fastest nameserver, which causes a big speedup!
-   LDAP now has different lookup models
-   Cleanups, better load distribution, better exception handling, zone2ldap improvements
-   The recursor was somewhat chatty about TCP connections
-   PostgreSQL now only depends on the C API and not on the deprecated C++ one
-   PowerDNS can now fully overrule external zones when doing recursion. See [Recursion](authoritative/recursion.md "Recursion").

# Version 2.9.13

Big news! Windows is back! Our great friend Michel Stol found the time to update the PowerDNS code so it works again under windows.

Furthermore, big thanks go out to Dell who quickly repaired my trusty [laptop](http://ds9a.nl/dell-d800).

His changes
-   Generic SQLite support added
-   Removed the ODBC backend, replaced it by the Generic ODBC Backend, which has all the cool configurability of the Generic MySQL and PostgreSQL backends.
-   The PowerDNS Recursor now runs as a Service. It defaults to running on port 5300, PowerDNS itself is configured to expect the Recursor on port 5300 now.
-   The PowerDNS Service is now known as 'PowerDNS' to Windows.
-   The Installer was redone, this time with [NSIS2](http://nsis.sf.net).
-   General updates and fixes.

## Other news
**Note**: There appears to be a problem with PowerDNS on Red Hat 7.3 with GCC 2.96 and self-compiled binaries. The symptoms are that PowerDNS works on the foreground but fails as a daemon. We're working on it.

If you do note problems, let the list know, if you don't, please do so as well. Tell us if you use the RPM or compiled yourself.

It is known that not compiling in MySQL support helps solve the problem, but then you don't have MySQL.

There have been a number of reports on MySQL connections being dropped on FreeBSD 4.x, which sometimes causes PowerDNS to give up and reload itself. To combat this, MySQL error messages have been improved in some places in hopes of figuring out what is up. The initial indication is that MySQL itself sometimes terminates the connection and, amazingly, that switching to a Unix domain socket instead of TCP solves the problem.

## Bug fixes
-   **allow-axfr-ips** did not work for individual IP addresses (bug & fix by Norbert Sendetzky)

## Improvements
-   Opteron support! Thanks to Jeff Davey for providing a shell on an Opteron. The fixes should also help PowerDNS on other platforms with a 64 bit userspace.

    Btw, the PowerDNS team has a strong desire for an Opteron :-)

-   pdns\_recursor jumbles answers now. This means that you can do poor man's round robin by supplying multiple A, MX or AAAA records for a service, and get a random one on top each time. Interestingly, this feature appeared out of nowhere, this change was made to the authoritative code but due to the wonders of code-reuse had an effect on pdns\_recursor too.
-   Big LDAP cleanup. Support for TLS was added. Zone2LDAP also gained the ability to generate ldif files containing a tree or a list of entries. (Norbert Sendetzky)
-   Zone2sql is now somewhat clearer when reporting malformed line errors - it did not always include the name of the file causing a problem, especially for big installations. Problem noted by Thom May.
-   pdns\_recursor now survives the expiration of all its root records, most often caused by prolonged disconnection from the net.

# Version 2.9.12

Release rich in features. Work on Verisign oddities, addition of SQLite backend, pdns\_recursor maturity.

## New features
-   --version command (requested by Mike Benoit)
-   delegation-only, a Verisign special.
-   Generic [SQLite](http://www.sqlite.org) support, by Michel 'Who da man?' Stol. See [Generic SQLite backend](authoritative/backend-generic-sqlite.md).
-   init.d script for pdns\_recursor
-   Recursor now actually purges its cache, saving memory.
-   Slave configuration now no longer falls over when presented with a NULL master
-   Bindbackend2 now has supermaster support (Mark Bergsma, untested)
-   Answers are now shuffled! It turns out a few recursors don't do shuffling (pdns\_recursor, djbdns), so we do it now. Requested by Jorn Ekkelenkamp of ISP-Services. This means that if you have multiple IP addresses for one host, they will be returned in differing order every once in a while.

## Bugs
-   0.0.0.0/0 didn't use to work (Norbert Sendetzky)
-   pdns\_recursor would try to resolve IP address which to bind to, potentially causing chicken/egg problem
-   gpgsql no longer reports as gmysql (Sherwin Daganoto)
-   SRV would not be parsed right from disk (Christof Meerwald)
-   An AXFR from a zone hosted on the LDAP backend no longer transmits all the reverse entries too (Norbert Sendetzky)
-   PostgreSQL backend now does error checking. It would be a bit too trusting before.

## Improvements, cleanups
-   PowerDNS now reports the numerical IP addresses it binds to instead of the, possibly, alphanumeric names the operator passed.
-   Removed only-soa hackery (noticed by Norbert Sendetzky)
-   Debian packaging fixes (Wichert Akkerman)
-   Some parameter descriptions were improved.
-   Cleanups by Norbert: getAuth moved to chopOff, arguments::contains massive cleanup, more.

# Version 2.9.11
Yet another iteration, hopefully this will be the last silly release.

**Warning**: There has been a change in behaviour whereby **disable-axfr** does what it means now! From now on, setting **allow-axfr-ips** automatically disables AXFR from unmentioned subnets.

This release enables AXFR again, **disable-axfr** did the opposite of what it claimed. Furthermore, the pdns\_recursor now cleans its cache, which should save some memory in the long run. Norbert contributed some small LDAP work which should come in useful in the future.

# Version 2.9.10
Small bugfixes, LDAP update. Released 3rd of July 2003. Apologies for the long delay, real life keeps interfering.

**Warning**: Do not use or try to use 2.9.9, it was a botched release!

**Warning**: There has been a change in behaviour whereby **disable-axfr** does what it means now! From now on, setting **allow-axfr-ips** automatically disables AXFR from unmentioned subnets.

-   2.9.8 was prone to crash on adding additional records. Thanks to excellent debugging by PowerDNS users worldwide, the bug was found quickly and is in fact present in all earlier PowerDNS releases, but for some reason doesn't cause crashes there.
-   Notifications now jump in front of the queue of domains that need to be checked for changes, giving much greater perceived performance. This is needed if you have tens of thousands of slave domains and your master server is on a high latency link. Thanks to Mark Jeftovic of EasyDNS for suggesting this change and testing it on their platform.
-   Dean Mills reported that PowerDNS does confusing logging about changing GIDs and UIDs, fixed. Cosmetic only.
-   pdns\_recursor may have logged empty lines for some users, fixed. Solution suggested by Norbert Sendetzky.
-   LDAP: DNS TTLs were random values (Norbert Sendetzky, Stefan Pfetzing). New **ldap-default-ttl** option.
-   LDAP: Now works with OpenLDAP 2.1 (Norbert Sendetzky)
-   LDAP: error handling for invalid MX records implemented (Norbert Sendetzky)
-   LDAP: better exception handling (Norbert Sendetzky)
-   LDAP: code cleanup of lookup() (Norbert Sendetzky)
-   LDAP: added support for scoped searches (Norbert Sendetzky)

# Version 2.9.8
Queen's day release! 30th of April 2003.

Added support for AIX, fixed negative SOA caching. Some other cleanups. Not a major release but enough reasons to upgrade.

## Bugs fixed
-   Recursor had problems expiring negatively cached entries, which wasted memory and also led to the continued non-existence of hosts that since had come into existence.
-   The Generic SQL backends did not lowercase the names of records, which led to new records not being found by case sensitive databases (notably PostgreSQL). Found by Volker Goetz.
-   NS queries for zones for which we did not carry authority, but only had delegation information, had their NS records in the wrong section. Minor detail, but a standards violation nonetheless. Spotted by Stephane Bortzmeyer.

## Improvements
-   Removed crypt.h dependency from powerldap.hh, which was a problem on some platforms (Richard Arends)
-   PowerDNS can't parse so called binary labels which we now detect and ignore, after printing a warning.
-   Specifying allow-axfr-ips now automatically disables AXFR for all non-mentioned addresses.
-   A Solaris ready init.d script is now part of the tar.gz (contributed, but I lost by whom).
-   Added some fixes to PowerDNS can work on AIX (spotted by Markus Heimhilcher).
-   Norbert Sendetzky contributed `zone2ldap`.
-   Everybody's favorite compiler warning from `zone2sql.cc` was removed!
-   Recursor now listens on TCP!

# Version 2.9.7
Released on 2003-03-20.

This is a sweeping release in the sense of cleanup. There are some new features but mostly a lot of cleanup going on. Hiding inside is the `bind2backend`, the next generation of the bind backend. A work in progress. Those of you with overlapping zones, as mentioned in the changelog of 2.9.6, are invited to check it out by replacing **launch=bind** by **launch=bind2** and renaming all **bind-** parameters to **bind2-**. Be aware that if you run with many small zones, this backend is faster, but if you run with a few large ones, it is slower. This will improve.

## Features
-   Mark Bergsma contributed **query-local-address** which allows the operator to select which source address to use. This is useful on servers with multiple source addresses and the operating system selecting an unintended one, leading to remotes denying access.
-   PowerDNS can now perform AAAA additional processing optionally, turned on by setting **do-ipv6-additional-processing**. Thanks to Stephane Bortzmeyer for pointing out the need.
-   Bind2backend, which is almost in compliance with the new IETF AXFR-clarify (some would say 'redefinition') draft.
    This backend is not ready for primetime but you may want to try it if you currently have overlapping zones and note problems. An overlapping zone would be having "ipv6.powerdns.com" and "powerdns.com" zones on one server.

## Improvements
-   Zone2sql would happily try to read from a directory and not give a useful error about this.
-   PowerDNS now reports the case where it can't figure out any IP address of slave nameservers for a zone
-   Removed **receiver-threads** setting which was experimental and in fact only made things worse.
-   LDAP backend updates from its author Norbert Sendetzky. Reverse lookups should work now too.
-   An error message about unparseable packets did not include the originating IP address (fixed by Mark Bergsma)
-   PowerDNS can now be started via path resolution while running with a guardian. Suggested by Maurice Nonnekes.
-   `pdns_recursor` moved to `sbin` (reported by Norbert Sendetzky)
-   Retuned some logger errorlevels, a lot of master/slave chatter was logged as 'Error'. Reported by Willem de Groot.

## Bugs fixed
-   `zone2sql` did not remove trailing dots in SOA records.
-   ldapbackend did not include `utility.hh` which caused compilation problems on Solaris (reported by Remco Post)
-   `pdns_control` could leave behind remnants in case PowerDNS was not running (reported by dG)
-   Incoming AXFR did not work on Solaris and other big-endian systems (Willem de Groot helped debugging this long standing problem).
-   Recursor could crash on convoluted CNAME loops. Thanks to Dan Faerch for delivering core dumps.
-   Silly 'wuh' debugging output in zone2sql and bindbackend removed (spotted by Ivo van der Wijk).
-   Recursor neglected to differentiate between negative cache of NXDOMAIN and NOERROR, leading to problems with IPv6 enabled Windows clients. Thanks to Stuart Walsh for reporting this and testing the fix.
-   PowerDNS set the 'aa' bit on serving NS records in a zone for which it was authoritative. Most implementations drop the 'aa' bit in this case and Stephane Bortzmeyer informed us of this. PowerDNS now also drops the 'aa' bit in this case.
-   The webserver tended to fail after prolonged operation on FreeBSD, this was due to an uninitialised timeout, other platforms were lucky. Thanks to G.P. de Boer for helping debug this.
-   getAnswers() in dnspacket.cc could be forced to read bytes beyond the end of the packet, leading to crashes in the PowerDNS recursor. This is an ongoing project that needs more work. Reported by Dan Faerch, with a core dump proving the problem.

# Version 2.9.6
Two new backends - Generic ODBC (windows only) and LDAP. Furthermore, a few important bugs have been fixed which may have hampered sites seeing a lot of outgoing zone transfers. Additionally, the pdns recursor now has 'query throttling' which is pretty cool. In short this makes sure that PowerDNS does not send out heaps of queries if a nameserver is unable to provide an answer. Many operators of authoritative setups are all too aware of recursing nameservers that hammer them for zones they don't have, PowerDNS won't do that anymore now, no matter what clients request of it.

**Warning**: There is an unresolved issue with the BIND backend and 'overlapping' slave zones. So if you have 'example.com' and also have a separate slave zone called 'external.example.com', things may go wrong badly. Thanks to Christian Laursen for working with us a lot in finding this issue. We hope to resolve it soon.

-   BIND Backend now honours notifies, code to support this was accidentally left out. Thanks to Christian Laursen for noticing this.
-   Massive speedup for those of you using the slightly deprecated MBOXFW records. Thanks to Jorn of [ISP Services](http://www.ISP-Services.nl) for helping and testing this improvement.
-   $GENERATE had an off-by-one bug where it would omit the last record to be generated (Christian Laursen)
-   Simultaneous AXFRs may have been problematic on some backends. Thanks to Jorn of ISP-Services again for helping us resolve this issue.
-   Added LDAP backend by Norbert Sendetzky, see [LDAP Backend](authoritative/backend-ldap.md).
-   Added Generic ODBC backend for Windows by Michel Stol.
-   Simplified 'out of zone data' detection in incoming AXFR support, hopefully removing a case sensitivity bug there. Thanks again to Christian Laursen for reporting this issue.
-   $include in-zonefile was broken under some circumstances, losing the last character of a file name. Thanks to Joris Vandalon for noticing this.
-   The zone parser was more case-sensitive than BIND, refusing to accept 'in' as well as 'IN'. Thanks to Joris Vandalon for noticing this.

# Version 2.9.5
Released on 2002-02-03.

This version is almost entirely about recursion with major changes to both the pdns recursor, which is renamed to '`pdns_recursor`' and to the main PowerDNS binary to make it interact better with the recursing component.

Sadly, due to [technical reasons](http://sources.redhat.com/ml/libc-alpha/2003-01/msg00245.html), compiling the pdns recursor and pdns authoritative nameserver into one binary is not immediately possible. During the release of 2.9.4 we stated that the recursing nameserver would be integrated in the next release - this won't happen now.

However, this turns out to not be that bad at all. The recursor can now be restarted without having to restart the rest of the nameserver, for example. Cooperation between the both halves of PowerDNS is also almost seamless. As a result, 'non-lazy recursion' has been dropped. See [Recursion](authoritative/recursion.md "Recursion") for more details.

Furthermore, the recursor only works on Linux, Windows and Solaris (not entirely). FreeBSD does not support the required functions. If you know any important FreeBSD people, plea with them to support set/get/swapcontext! Alternatively, FreeBSD coders could read the solution presented here [in figure 5](http://www.eng.uwaterloo.ca/~ejones/software/threading.html).

The 'Contributor of the Month' award goes to Mark Bergsma who has responded to our plea for help with the label compressor and contributed a wonderfully simple and right fix that allows PowerDNS to compress just as well as other nameservers out there. An honorary mention goes to Ueli Heuer who, despite having no C++ experience, submitted an excellent SRV record implementation.

Excellent work was also performed by Michel Stol, the Windows guy, in fixing all our non-portable stuff again. Christof Meerwald has also done wonderful work in porting MTasker to Windows, which was then used by Michel to get the recursor functioning on Windows.

## Other changes
-   dnspacket.cc was cleaned up by factoring out common operations
-   Heaps of work on the recursing nameserver. Has now achieved *days* of uptime!
-   Recursor renamed from syncres to `pdns_recursor`
-   PowerDNS can now serve records it does not know about. To benefit from this slightly undocumented feature, add 1024 to the numerical type of a record and include the record in binary form in your database. Used internally by the recursing nameserver but you can use it too.
-   PowerDNS now knows about SIG and KEY records *names*. It does not support them yet but can at least report so now.
-   HINFO records can now be transferred from a master to PowerDNS (thanks to Ueli Heuer for noticing it didn't work).
-   Yet more UltraSPARC alignment issues fixed (Chris Andrews).
-   Dropped non-lazy recursion, nobody was using it. Lazy recursion became even more lazy after Dan Bernstein pointed out that additional processing is not vital, so PowerDNS does its best to do additional processing on recursive queries, but does not scream murder if it does not succeed. Due to caching, the next identical query will be successfully additionally processed.
-   Label compression was improved so we can now fit all . records in 436 bytes, this used to be 460! (Code & formal proof of correctness by Mark Bergsma).
-   SRV support (incoming and outgoing), submitted by Ueli Heuer.
-   Generic backends do not support SOA serial autocalculation, it appears. Could lead to random SOA serials in case of a serial of 0 in the database. Fixed so that 0 stays zero in that case. Don't set the SOA serial to 0 when using Generic MySQL or Generic PostgreSQL!
-   J root-server address was updated to its new location.
-   SIGUSR1 now forces the recursor to print out statistics to the log.
-   Meaning of recursor logging was changed a bit - a cache hit is now a question that was answered with 0 outgoing packets needed. Used to be a weighted average of internal cache hits.
-   MySQL compilation did not include -lz which causes problems on some platforms. Thanks to James H. Cloos Jr for reporting this.
-   After a suggestion by Daniel Meyer and Florus Both, the built in webserver now reports the configuration name when multiple PowerDNS instances are active.
-   Brad Knowles noticed that zone2sql had problems with the root.zone, fixed. This also closes some other zone2sql annoyances with converting single zones.

# Version 2.9.4
Yet another grand release. Big news is the addition of a recursing nameserver which has sprung into existence over the past week. It is in use on several computers already but it is not ready for prime time. Complete integration with PowerDNS is expected around 2.9.5, for now the recursor is a separate program.

In preliminary tests, the recursor appears to be four times faster than BIND 9 on a naive benchmark starting from a cold cache. BIND 9 managed to get through to some slower nameservers however, which were given up on by PowerDNS. We will continue to tune the recursor. See [PowerDNS Recursor](recursor/index.md) for further details.

The BIND Backend has also been tested (see the **bind-domain-status** item below) rather heavily by several parties. After some discussion online, one of the BIND authors ventured that the newsgroup comp.protocols.dns.bind may now in fact be an appropriate venue for discussing PowerDNS. Since this discussion, traffic to the PowerDNS pages has increased sixfold and shows no signs of slowing down.

From this, it is apparent that far more people are interested in PowerDNS than yet know about it. So spread the word!

In other news, we now have a security page at [Security](security/index.md). Furthermore, Maurice Nonnekes contributed an OpenBSD port! See [his page](http://www.codeninja.nl/openbsd/powerdns/) for more details!

## New features and improvements
-   All SQL queries in the generic backends are now available for configuration. (Martin Klebermass, Bert Hubert). See [Generic SQL backends](authoritative/backend-generic-sql.md).
-   A recursing nameserver! See [PowerDNS Recursor](recursor/index.md).
-   An incoming AXFR now only starts a backend zone replacement transaction after the first record arrived successfully, thus making sure no work is done when a remote nameserver is unable/unwilling to AXFR a zone to us.
-   Zone parser error messages were improved slightly (thanks to Stef van Dessel for spotting this shortcoming)
-   XS4ALL's Erik Bos checked how PowerDNS reacted to a BIND installation with almost 60.000 domains, some of which with \>100.000 records, and he discovered the pdns\_control **bind-domain-status** command became very slow with larger numbers of domains. Fixed, 60.000 domains are now listed in under one second.
-   If a remote nameserver disconnects during an incoming AXFR, the update is now rolled back, unless the AXFR was properly terminated.
-   The migration chapter mentioned the use of deprecated backends.

## A tremendous number of bugs were discovered and fixed
-   Zone parser would only accept $include and not $INCLUDE
-   Zone parser had problems with $lines with comments on the end
-   Wildcard ANY queries were broken (thanks Colemarcus for spotting this)
-   A connection failure with the Generic backends would lead to a powerdns reload (cast of many)
-   Generic backends had some semantic problems with slave support. Symptoms were oft-repeated notifications and transfers (thanks to Mark Bergsma for helping resolve this).
-   Solaris version compiles again. Thanks to Mohamed Lrhazi for reporting that it didn't.
-   Some UltraSPARC alignment fixes. Thanks to Mohamed Lrhazi for being helpful in spotting these. One problem is still outstanding, Mohamed sent a core dump that tells us where the problem is. Expect the fix to be in 2.9.5. Volunteers can grep the source for 'UltraSPARC' to find where the problem is.
-   Our support of IPv6 on FreeBSD had phase of moon dependent bugs, fixed by Peter van Dijk.
-   Some crashes of and by pdns\_control were fixed, thanks to Mark Bergsma for helping resolve these.
-   Outgoing AXFR in pdns installations with multiple loaded backends was broken (thanks to Stuart Walsh for reporting this).
-   A failed BIND Backend incoming AXFR would block the zone until it succeeded again.
-   Generic PostgreSQL backend wouldn't compile with newer libpq++, fixed by Julien Lemoine/SpeedBlue.
-   Potential bug (not observed) when listening on multiple interfaces fixed.
-   Some typos in manpages fixed (reported by Marco Davids).

# Version 2.9.3a

**Note**: 2.9.3a is identical to 2.9.3 except that zone2sql does work

Broad range of huge improvements. We now have an all-static .rpm and .deb for Linux users and a link to an OpenBSD port. Major news is that work on the Bind backend has progressed to the point that we've just retired our last Bind server and replaced it with PowerDNS in Bind mode! This server is operating a number of master and slave setups so it should stress the Bind backend somewhat.

This version is rapidly approaching the point where it is a better-Bind-than-Bind and nearly a drop-in replacement for authoritative setups. PowerDNS is now equipped with a powerful master/slave apparatus that offers a lot of insight and control to the user, even when operating from Bind zone files and a Bind configuration. Observe.

After the SOA of example.org was raised

```
pdns[17495]: All slave domains are fresh
pdns[17495]: 1 domain for which we are master needs notifications
pdns[17495]: Queued notification of domain 'example.org' to 195.193.163.3
pdns[17495]: Queued notification of domain 'example.org' to 213.156.2.1
pdns[17520]: AXFR of domain 'example.org' initiated by 195.193.163.3
pdns[17520]: AXFR of domain 'example.org' to 195.193.163.3 finished
pdns[17521]: AXFR of domain 'example.org' initiated by 213.156.2.1
pdns[17521]: AXFR of domain 'example.org' to 213.156.2.1 finished
pdns[17495]: Removed from notification list: 'example.org' to 195.193.163.3 (was acknowledged)
pdns[17495]: Removed from notification list: 'example.org' to 213.156.2.1 (was acknowledged)
pdns[17495]: No master domains need notifications
```

If however our slaves would ignore us, as some are prone to do, we can send some additional notifications

```
$ sudo pdns_control notify example.org
Added to queue
pdns[17492]: Notification request for domain 'example.org' received
pdns[17492]: Queued notification of domain 'example.org' to 195.193.163.3
pdns[17492]: Queued notification of domain 'example.org' to 213.156.2.1
pdns[17495]: Removed from notification list: 'example.org' to 195.193.163.3 (was acknowledged)
pdns[17495]: Removed from notification list: 'example.org' to 213.156.2.1 (was acknowledged)
```

Conversely, if PowerDNS needs to be reminded to retrieve a zone from a master, a command is provided

```
$ sudo pdns_control retrieve forfun.net
Added retrieval request for 'forfun.net' from master 212.187.98.67
pdns[17495]: AXFR started for 'forfun.net', transaction started
pdns[17495]: Zone 'forfun.net' (/var/cache/bind/forfun.net) reloaded
pdns[17495]: AXFR done for 'forfun.net', zone committed
```

Also, you can force PowerDNS to reload a zone from disk immediately with **pdns\_control bind-reload-now**. All this happens 'live', per your instructions. Without instructions, the right things also happen, but the operator is in charge.

For more about all this coolness, see [“pdns\_control”](authoritative/running.md#pdnscontrol "pdns_control") and [“pdns\_control commands”](authoritative/backend-bind.md#bind-control-commands "pdns_control commands").

**Warning**: Again some changes in compilation instructions. The hybrid pgmysql backend has been split up into 'gmysql' and 'gpgsql', sharing a common base within the PowerDNS server itself. This means that you can no longer compile **--with-modules="pgmysql" --enable-mysql --enable-pgsql** but that you should now use: **--with-modules="gmysql gpgsql"**. The old launch-names remain available.

If you launch the Generic PostgreSQL backend as gpgsql2, all parameters will have gpgsql2 as a prefix, for example **gpgsql2-dbname**. If launched as gpgsql, the regular names are in effect.

**Warning**: The pdns\_control protocol was changed which means that older pdns\_controls cannot talk to 2.9.3. The other way around is broken too. This may lead to problems with automatic upgrade scripts, so pay attention if your daemon is truly restarted.

Also make sure no old pdns\_control command is around to confuse things.

## Improvements
-   Bind backend can now deal with missing files and try to find them later.
-   Bind backend is now explicitly master capable and triggers the sending of notifications.
-   General robustness improvements in Bind backend - many errors are now non-fatal.
-   Accessibility, Serviceability. New **pdns\_server** commands like **bind-list-rejects** (lists zones that could not be loaded, and the reason why), **bind-reload-now** (reload a zone from disk NOW), **rediscover** (reread named.conf NOW). More is coming up.
-   Added support for retrieving RP (Responsible Person) records from remote masters. Serving them was already possible.
-   Added support for LOC records, which encode the geographical location of a host, both serving and retrieving (thanks to Marco Davids using them on our last Bind server, forcing us to implement this silly record).
-   Configuration file parser now strips leading spaces too, allowing "chroot= /tmp" to work, as well as "chroot=/tmp" (Thanks to Hub Dohmen for reporting this for months on end).
-   Added **bind-domain-status** command that shows the status of all domains (when/if they were parsed, any errors encountered while parsing them).
-   Added **bind-reload-now** command that tries to reload a zone from disk NOW, and reports back errors to the operator immediately.
-   Added **retrieve** command that queues a request to retrieve a zone from its master.
-   Zones retrieved from masters are now stored way smaller on disk because the domain is stripped from records, which is derived from the configuration file. Retrieved zones are now prefixed with some information on where they came from.

## Changes
-   gpgsql and gmysql backends split out of the hybrid pgmysqlbackend. This again changed compilation instructions!
-   **pdns\_control** now uses the rarely seen SOCK\_STREAM Unix Domain socket variety so it can transport large amounts of text, which is needed for the **bind-domain-status** command, for which see [Pdns\_control commands](authoritative/backend-bind.md#bind-control-commands "Pdns_control commands"). This breaks compatibility with older pdns\_control and pdns\_server binaries!
-   Bind backend now ignores 'hint' and 'forward' and other unsupported zone types.
-   AXFRs are now logged more heavily by default. An AXFR is a heavy operation anyhow, some more logging does not further increase the load materially. Does help in clearing up what slaves are doing.
-   A lot of master/slave chatter has been silenced, making output more relevant. No more repetitive 'No master domains need notifications' etc, only changes are reported now.

## Bugfixes
-   Windows version did not compile without minor changes.
-   Confusing error reporting on Windows 98 (which does not support PowerDNS) fixed
-   Potential crashes with shortened packets addressed. An upgrade is advised!
-   **notify** (which was already there, just badly documented) no longer prints out debugging garbage.
-   pgmysql backend had problems launching when not compiled in but available as a module. Workaround for 2.9.2 is 'load-modules=pgmysql', but even then gpgsql would not work! gmysql would then, however. These modules are now split out, removing such issues.

# Version 2.9.2
Bugfixes galore. Solaris porting created some issues on all platforms. Great news is that PowerDNS is now in Debian 'sid' (unstable). The 2.9.1 packages in there currently aren't very good but the 2.9.2 ones will be. Many thanks to Wichert Akkerman, our 'downstream' for making this possible.

**Warning**: The Generic MySQL backend, part of the Generic MySQL & PostgreSQL backend, is now the DEFAULT! The previous default, the 'mysql' backend (note the lack of 'g') is now DEPRECATED. This was the source of much confusion. The 'mysql' backend does not support MASTER or SLAVE operation. The Generic backends do.

To get back the mysql backend, add --with-modules="mysql" or --with-dynmodules="mysql" if you prefer to load your modules at runtime.

## Bugs fixed
-   Silly debugging output removed from the webserver (found by Paul Wouters)
-   SEVERE: due to Solaris portability fixes, qtypes\<127 were broken. These include NAPTR, ANY and AXFR. The upshot is that powerdns wasn't performing outgoing AXFRs nor ANY queries. These were the 'question for type -1' warnings in the log
-   incoming AXFR could theoretically miss some trailing records (not observed, but could happen)
-   incoming AXFR did not support TXT records (spotted by Paul Wouters)
-   with some remotes, an incoming AXFR would not terminate until a timeout occurred (observed by Paul Wouters)
-   Documentation bug, pgmysql != mypgsql

## Documentation
-   Documented the 'random backend', see [Random Backend](authoritative/backend-random.md "Random Backend").
-   Wichert Akkerman contributed three manpages.
-   Building PowerDNS on Unix is now documented somewhat more, see [Compiling PowerDNS on Unix](appendix/compiling-powerdns.md#on-unix "Compiling PowerDNS on Unix").

## Features
-   pdns init.d script is now +x by default
-   OpenBSD is on its way of becoming a supported platform! As of 2.9.2, PowerDNS compiles on OpenBSD but swiftly crashes. Help is welcome.
-   ODBC backend (for Windows only) was missing from the distribution, now added.
-   xdb backend added - see [XDB Backend](authoritative/backend-deprecated.md#xdb-backend). Designed for use by root-server operators.
-   Dynamic modules are back which is good news for distributors who want to make a pdns packages that does not depend one every database under the sun.

# Version 2.9.1
Thanks to the great enthusiasm from around the world, powerdns is now available for Solaris and FreeBSD users again! Furthermore, the Windows build is back. We are very grateful for the help of

-   Michel Stol
-   Wichert Akkerman
-   Edvard Tuinder
-   Koos van den Hout
-   Niels Bakker
-   Erik Bos
-   Alex Bleker
-   Steven Stillaway
-   Roel van der Made
-   Steven Van Steen

We are happy to have been able to work with the open source community to improve PowerDNS!

## Changes
-   The monitor command **set** no longer allows the changing of non-existent variables.
-   IBM Universal Database DB2 backend now included in source distribution (untested!)
-   Oracle backend now included in source distribution (slightly tested!)
-   configure script now searches for postgresql and mysql includes
-   Bind parser now no longer dies on records with a ' in them (Erik Bos)
-   The pipebackend was accidentally left out of 2.9
-   FreeBSD fixes (with help from Erik Bos, Alex Bleeker, Niels Bakker)
-   Heap of Solaris work (with help from Edvard Tuinder, Stefan Van Steen, Koos van den Hout, Roel van der Made and especially Mark Bakker). Now compiles in 2.7 and 2.8, haven't tried 2.9. May be a bit dysfunctional on 2.7 though - it won't do IPv6 and it won't serve AAAA. Patches welcome!
-   Windows 32 build is back! Michel Stol updated his earlier work to the current version.
-   S/Linux (Linux on Sparc) build works now (with help from Steven Stillaway).
-   Silly debugging message ('sd.ttl from cache') removed
-   .deb files are back, hopefully in 'sid' soon! (Wichert Akkerman)
-   Removal of bzero and other less portable constructs. Discovered that recent Linux glibc's need -D\_GNU\_SOURCE (Wichert Akkerman).

# Version 2.9
Open source release. Do not deploy unless you know what you are doing. Stability is expected to return with 2.9.1, as are the binary builds.

-   License changed to the GNU General Public License version 2.
-   Cleanups by Erik Bos @ xs4all.
-   Build improvements by Wichert Akkerman
-   Lots of work on the build system, entirely revamped. By PowerDNS.

# Version 2.8
From this release onwards, we'll concentrate on stabilising for the 3.0 release. So if you have any must-have features, let us know soonest. The 2.8 release fixes a bunch of small stability issues and add two new features. In the spirit of the move to stability, this release has already been running 24 hours on our servers before release.

-   pipe backend gains the ability to restricts its invocation to a limited number of requests. This allows a very busy nameserver to still serve packets from a slow perl backend.
-   pipe backend now honors query-logging, which also documents which queries were blocked by the regex.
-   pipe backend now has its own backend chapter.
-   An incoming AXFR timeout at the wrong moment had the ability to crash the binary, forcing a reload. Thanks to our bug spotting champions Mike Benoit and Simon Kirby of NetNation for reporting this.

# Version 2.7 and 2.7.1
This version fixes some very long standing issues and adds a few new features. If you are still running 2.6, upgrade yesterday. If you were running 2.6.1, an upgrade is still strongly advised.

## Features
-   The controlsocket is now readable and writable by the 'setgid' user. This allows for non-root access to PowerDNS which is nice for mrtg or cricket graphs.
-   MySQL backend (the non-generic one) gains the ability to read from a different table using the **mysql-table** setting.
-   pipe backend now has a configurable timeout using the **pipe-timeout** setting. Thanks to Steve Bromwich for pointing out the need for this.
-   Experimental backtraces. If PowerDNS crashes, it will log a lot of numbers and sometimes more to the syslog. If you see these, please report them to us. Only available under Linux.

## Bugs
-   2.7 briefly broke the mysql backend, so don't use it if you use that. 2.7.1 fixes this.
-   SOA records could sometimes have the wrong TTL. Thanks to Jonas Daugaard for reporting this.
-   An ANY query might lead to duplicate SOA records being returned under exceptional circumstances. Thanks to Jonas Daugaard for reporting this.
-   Underlying the above bug, packet compression could sometimes suddenly be turned off, leading to overly large responses and non-removal of duplicate records.
-   The **allow-axfr-ips** setting did not accept IP ranges (192.0.2.0/24) which the documentation claimed it did (thanks to Florus Both of Ascio technologies for being sufficiently persistent in reporting this).
-   Killed backends were not being respawned, leading to suboptimal behaviour on intermittent database errors. Thanks to Steve Bromwich for reporting this.
-   Corrupt packets during an incoming AXFR when acting as a slave would cause a PowerDNS reload instead of just failing that AXFR. Thanks to Mike Benoit and Simon Kirby of NetNation for reporting this.
-   Label compression in incoming AXFR had problems with large offsets, causing the above mentioned errors. Thanks to Mike Benoit and Simon Kirby of NetNation for reporting this.

# Version 2.6.1

Quick fix release for a big cache problem.

# Version 2.6
Performance release. A lot of work has been done to raise PowerDNS performance to staggering levels in order to take part in benchmarketing efforts. Together with our as yet unnamed partner, PowerDNS has been benchmarked at 60.000 mostly cached queries/second on off the shelf PC hardware. Uncached performance was 17.000 uncached DNS queries/second on the .ORG domain.

Performance has been increased by both making PowerDNS itself quicker but also by lowering the number of backend queries typically needed. Operators will typically see PowerDNS taking less CPU and the backend seeing less load.

Furthermore, some real bugs were fixed. A couple of undocumented performance switches may appear in --help output but you are advised to stay away from these.

Developers: this version needs the pdns-2.5.1 development kit, available on <http://downloads.powerdns.com/releases/dev>. See also [Backend writers' guide](appendix/backend-writers-guide.md "Backend writers' guide").

## Performance
-   A big error in latency calculations - cached packets were weighed 50 times less, leading to inflated latency reporting. Latency calculations are now correct and way lower - often in the microseconds range.
-   It is now possible to run with 0 second cache TTLs. This used to cause very frequent cache cleanups, leading to performance degradation.
-   Many tiny performance improvements, removing duplicate cache key calculations, etc. The cache itself has also been reworked to be more efficient.
-   First 'CNAME' backend query replaced by an 'ANY' query, which most of the time returns the actual record, preventing the need for a separate CNAME lookup, halving query load.
-   Much of the same for same-level-NS records on queries needing delegation.

## Bugs fixed
-   Incidentally, the cache count would show 'unknown' packets, which was harmless but confusing. Thanks to Mike and Simon of NetNation for reporting this.
-   SOA hostmaster with a . in the local-part would be cached wrongly, leading to a stray backslash in case of multiple successively SOA queries. Thanks to Ascio Technologies for spotting this bug.
-   zone2sql did not parse Verisign zone files correctly as these contained a $TTL statement in mid-record.
-   Sometimes packets would not be accounted, leading to 'udp-queries' and 'udp-answers' divergence.

## Features
-   'cricket' command added to init.d scripts that provides unadorned output for parsing by 'Cricket'.

# Version 2.5.1
[Brown paper bag](http://www.tuxedo.org/~esr/jargon/html/entry/brown-paper-bag-bug.html) release fixing a huge memory leak in the new Query Cache.

Developers: this version needs the new pdns-2.5.1 development kit, available on <http://downloads.powerdns.com/releases/dev>. See also [Backend writers' guide](appendix/backend-writers-guide.md "Backend writers' guide").

And some small changes

-   Added support for RFC 2308 compliant negative-answer caching. This allows remotes to cache the fact that a domain does not exist and will not exist for a while. Thanks to Chris Thompson for [pointing out how tiny our minds are](http://ops.ietf.org/lists/namedroppers/namedroppers.2002/msg01697.html). This feature may cause a noticeable reduction in query load.
-   Small speedup to non-packet-cached queries, incidentally fixing the huge memory leak.
-   **pdns\_control ccounts** command outputs statistics on what is in the cache, which is useful to help optimize your caching strategy.

# Version 2.5
An important release which has seen quite a lot of trial and error testing. As a result, PowerDNS can now run with a huge cache and concurrent invalidations. This is useful when running of a slower database or under high traffic load with a fast database.

Furthermore, the gpgsql2 backend has been validated for use and will soon supplant the gpgsql backend entirely. This also bodes well for the gmysql backend which is the same code.

Also, a large amount of issues biting large scale slave operators were addressed. Most of these issues would only show up after prolonged uptime.

## New features
-   Query cache. The old Packet Cache only cached entire questions and their answers. This is very CPU efficient but does not lead to maximum hitrate. Two packets both needing to resolve smtp.you.com internally would not benefit from any caching. Furthermore, many different DNS queries lead to the same backend queries, like 'SOA for .COM?'.

    PowerDNS now also caches backend queries, but only those having no answer (the majority) and those having one answer (almost the rest).

    In tests, these additional caches appear to halve the database backend load numerically and perhaps even more in terms of CPU load. Often, queries with no answer are more expensive than those having one.

    The default **ttl**s for the query-cache and negquery-cache are set to safe values (20 and 60 seconds respectively), you should be seeing an improvement in behaviour without sacrificing a lot in terms of quick updates.

    The webserver also displays the efficiency of the new Query Cache.

    The old Packet Cache is still there (and useful) but see [Authoritative Server Performance](authoritative/performance.md) for more details.

-   There is now the ability to shut off some logging at a very early stage. High performance sites doing thousands of queries/second may in fact spend most of their CPU time on attempting to write out logging, even though it is ignored by syslog. The new flag **log-dns-details**, on by default, allows the operator to kill most informative-only logging before it takes any cpu.
-   Flags which can be switched 'on' and 'off' can now also be set to 'off' instead of only to 'no' to turn them off.

## Enhancements
-   Packet Cache is now case insensitive, leading to a higher hitrate because identical queries only differing in case now both match. Care is taken to restore the proper case in the answer sent out.
-   Packet Cache stores packets more efficiently now, savings are estimated at 50%.
-   The Packet Cache is now asynchronous which means that PowerDNS continues to answer questions while the cache is busy being purged or queried. Incidentally this will mean a cache miss where previously the question would wait until the cache became available again.

    The upshot of this is that operators can call **pdns\_control purge** as often as desired without fearing performance loss. Especially the full, non-specific, purge was sped up tremendously.

    This optimization is of little merit for small sites but is very important when running with a large packetcache, such as when using recursion under high load.

-   AXFR log messages now all contain the word 'AXFR' to ease grepping.
-   Linux static version now compiled with gcc 3.2 which is known to output better and faster code than the previously used 3.0.4.

## Bugs fixed
-   Packetcache would sometimes send packets back with slightly modified flags if these differed from the flags of the cached copy.
-   Resolver code did bad things with file descriptors leading to fd exhaustion after prolonged uptimes and many slave SOA currency checks.
-   Resolver code failed to properly log some errors, leading to operator uncertainty regarding to AXFR problems with remote masters.
-   After prolonged uptime, slave code would try to use privileged ports for originating queries, leading to bad replication efficiency.
-   Masters sending back answers in differing case from questions would lead to bogus 'Master tried to sneak in out-of-zone data' errors and failing AXFRs.

# Version 2.4

Developers: this version is compatible with the pdns-2.1 development kit, available on <http://downloads.powerdns.com/releases/dev>. See also [*Backend writers' guide*](appendix/backend-writers-guide.md "Backend writers' guide").

This version fixes some stability issues with malformed or malcrafted packets. An upgrade is advised. Furthermore, there are interesting new features.

## New features
-   Recursive queries are now also cached, but in a separate namespace so non-recursive queries don't get recursed answers and vice versa. This should mean way lower database load for sites running with the current default lazy-recursion. Up to now, each and every recursive query would lead to a large amount of SQL queries.

    To prevent the packetcache from becoming huge, a separate **recursive-cache-ttl** can be specified.

-   The ability to change parameters at runtime was added. Currently, only the new **query-logging** flag can be changed.
-   Added **query-logging** flag which hints a backend that it should output a textual representation of queries it receives. Currently only gmysql and gpgsql2 honor this flag.
-   Gmysql backend can now also talk to PostgreSQL, leading to less code. Currently, the old postgresql driver ('gpgsql') is still the default, the new driver is available as 'gpgsql2' and has the benefit that it does query logging. In the future, gpgsql2 will become the default gpgsql driver.
-   DNS recursing proxy is now more verbose in logging odd events which may be caused by buggy recursing backends.
-   Webserver now displays peak queries/second 1 minute average.

## Bugs fixed
-   Failure to connect to database in master/slave communicator thread could lead to an unclean reload, fixed.

Documentation: added details for **strict-rfc-axfrs**. This feature can be used if very old clients need to be able to do zone transfers with PowerDNS. Very slow.

# Version 2.3

Developers: this version is compatible with the pdns-2.1 development kit, available on <http://downloads.powerdns.com/releases/dev>. See also [Backend writers' guide](appendix/backend-writers-guide.md "Backend writers' guide")

This release adds the Generic MySQL backend which allows full master/slave semantics with MySQL and InnoDB tables (or other tables that support transactions). See [Generic MySQL backend](authoritative/backend-generic-mysql.md "Generic MySQL backend").

## Other new features
-   Improved error messages in master/slave communicator will help down track problems.
-   **slave-cycle-interval** setting added. Very large sites with thousands of slave domains may need to raise this value above the default of 60. Every cycle, domains in indeterminate state are checked for their condition. Depending on the health of the masters, this may entail many SOA queries or attempted AXFRs.

## Bugs fixed
-   'pdns\_control purge **`domain`**' and 'pdns\_control purge **`domain$`**' were broken in version 2.2 and did not in fact purge the cache. There is a slight risk that domain-specific purge commands could force a reload in previous version. Thanks to Mike Benoit of NetNation for discovering this.
-   Master/slave communicator thread got confused in case of delayed answers from slow masters. While not causing harm, this caused inefficient behaviour when testing large amounts of slave domains because additional 'cycles' had to pass before all domains would have their status ascertained.
-   Backends implementing special SOA semantics (currently only the undocumented 'pdns express backend', or homegrown backends) would under some circumstances not answer the SOA record in case of an ANY query. This should put an end to the last DENIC problems. Thanks to DENIC for helping us find the problem.

# Version 2.2
Developers: this version is compatible with the pdns-2.1 development kit, available on <http://downloads.powerdns.com/releases/dev>. See also [Backend writers' guide](appendix/backend-writers-guide.md "Backend writers' guide")

Again a big release. PowerDNS is seeing some larger deployments in more demanding environments and these are helping shake out remaining issues, especially with recursing backends.

The big news is that wildcard CNAMEs are now supported, an oft requested feature and nearly the only part in which PowerDNS differed from BIND in authoritative capabilities.

If you were seeing signal 6 errors in PowerDNS causing reloads and intermittent service disruptions, please upgrade to this version.

For operators of PowerDNS Express trying to host .DE domains, the very special **soa-serial-offset** feature has been added to placate the new DENIC requirement that the SOA serial be at least six digits. PowerDNS Express uses the SOA serial as an actual serial and not to insert dates and hence often has single digit soa serial numbers, causing big problems with .DE redelegations.

## Bugs fixed

-   Malformed or shortened TCP recursion queries would cause a signal 6 and a reload. Same for EOF from the TCP recursing backend. Thanks to Simon Kirby and Mike Benoit of NetNation for helping debug this.
-   Timeouts on the TCP recursing backend were far too long, leading to possible exhaustion of TCP resolving threads.
-   **pdns\_control purge domain** accidentally cleaned all packets with that name as a prefix. Thanks to Simon Kirby for spotting this.
-   Improved exception error logging - in some circumstances PowerDNS would not properly log the cause of an exception, which hampered problem resolution.

## New features
-   Wildcard CNAMEs now work as expected!
-   **pdns\_control purge** can now also purge based on suffix, allowing operators to purge an entire domain from the packet cache instead of only specific records. See also [pdns\_control](authoritative/running.md#pdnscontrol "pdns_control") Thanks to Mike Benoit for this suggestion.
-   **soa-serial-offset** for installations with small SOA serial numbers wishing to register .DE domains with DENIC which demands six-figure SOA serial numbers. See also [Chapter 21, *Index of all Authoritative Server settings*](authoritative/settings.md "Index of all Authoritative Server settings").

# Version 2.1
This is a somewhat bigger release due to pressing demands from customers. An upgrade is advised for installations using Recursion. If you are using recursion, it is vital that you are aware of changes in semantics. Basically, local data will now override data in your recursing backend under most circumstances. Old behaviour can be restored by turning **lazy-recursion** off.

Developers: this version has a new pdns-2.1 development kit, available on <http://downloads.powerdns.com/releases/dev>. See also [Backend writers' guide](appendix/backend-writers-guide.md).

**Warning**: Most users will run a static version of PowerDNS which has no dependencies on external libraries. However, some may need to run the dynamic version. This warning applies to these users.

To run the dynamic version of PowerDNS, which is needed for backend drivers which are only available in source form, gcc 3.0 is required. RedHat 7.2 comes with gcc 3.0 as an optional component, RedHat 7.3 does not. However, the RedHat 7.2 Update gcc rpms install just fine on RedHat 7.3. For Debian, we suggest running 'woody' and installing the g++-3.0 package. We expect to release a FreeBSD dynamic version shortly.

## Bugs fixed
-   RPM releases sometimes overwrote previous configuration files. Thanks to Jorn Ekkelenkamp of Hubris/ISP Services for reporting this.
-   TCP recursion sent out overly large responses due to a byte order mistake, confusing some clients. Thanks to the capable engineers of NetNation for bringing this to our attention.
-   TCP recursion in combination with a recursing backend on a non-standard port did not work, leading to a non-functioning TCP listener. Thanks to the capable engineers of NetNation for bringing this to our attention.

## Unexpected behaviour
-   Wildcard URL records where not implemented because they are a performance penalty. To turn these on, enable **wildcard-url** in the configuration.
-   Unlike other nameservers, local data did not override the internet for recursing queries. This has mostly been brought into conformance with user expectations. If a recursive question can be answered entirely from local data, it is. To restore old behaviour, disable **lazy-recursion**. Also see [Recursion](authoritative/recursion.md "Recursion").

## Features
-   Oracle support has been tuned, leading to the first public release of the Oracle backend. Zone2sql now outputs better SQL and the backend is now fully documented. Furthermore, the queries are compatible with the PowerDNS XML-RPC product, allowing PowerDNS express to run off Oracle. See [Oracle backend](authoritative/backend-oracle.md "Oracle backend").
-   Zone2sql now accepts --transactions to wrap zones in a transaction for PostgreSQL and Oracle output. This is a major speedup and also makes for better isolation of inserts. See [Zone2sql](authoritative/migration.md#zone2sql "Zone2sql").
-   **pdns\_control** now has the ability to purge the PowerDNS cache or parts of it. This enables operators to raise the TTL of the Packet Cache to huge values and only to invalidate the cache when changes are made. See also [Authoritative Server Performance](authoritative/performance.md "Authoritative Server Performance") and [pdns\_control](authoritative/running.md#pdnscontrol "pdns_control").

# Version 2.0.1
Maintenance release, fixing three small issues.

Developers: this version is compatible with 1.99.11 backends.

-   PowerDNS ignored the **logging-facility** setting unless it was specified on the command line. Thanks to Karl Obermayer from WebMachine Technologies for noticing this.
-   Zone2sql neglected to preserve 'slaveness' of domains when converting to the slave capable PostgreSQL backend. Thanks to Mike Benoit of NetNation for reporting this. Zone2sql now has a **--slave** option.
-   SOA Hostmaster addresses with dots in them before the @-sign were mis-encoded on the wire.

# Version 2.0
Two bugfixes, one stability/security related. No new features.

Developers: this version is compatible with 1.99.11 backends.

Bugfixes
-   zone2sql refused to work under some circumstances, taking 100% cpu and not functioning. Thanks to Andrew Clark and Mike Benoit for reporting this.
-   Fixed a stability issue where malformed packets could force PowerDNS to reload. Present in all earlier 2.0 versions.

# Version 2.0 Release Candidate 2
Mostly bugfixes, no really new features.

Developers: this version is compatible with 1.99.11 backends.

## Bugs fixed
-   chroot() works again - 2.0rc1 silently refused to chroot. Thanks to Hub Dohmen for noticing this.
-   setuid() and setgid() security features were silently not being performed in 2.0rc1. Thanks to Hub Dohmen for noticing this.
-   MX preferences over 255 now work as intended. Thanks to Jeff Crowe for noticing this.
-   IPv6 clients can now also benefit from the recursing backend feature. Thanks to Andy Furnell for proving beyond any doubt that this did not work.
-   Extremely bogus code removed from DNS notification reception code - please test! Thanks to Jakub Jermar for working with us in figuring out just how broken this was.
-   AXFR code improved to handle more of the myriad different zone transfer dialects available. Specifically, interoperability with Bind 4 was improved, as well as Bind 8 in 'strict rfc conformance' mode. Thanks again for Jakub Jermar for running many tests for us. If your transfers failed with 'Unknown type 14!!' or words to that effect, this was it.

## Features
-   Win32 version now has a zone2sql tool.
-   Win32 version now has support for specifying how urgent messages should be before they go to the NT event log.

## Remaining issues
-   One persistent report of the default 'chroot=./' configuration not working.
-   One report of disable-axfr and allow-axfr-ips not working as intended.
-   Support for relative paths in zones and in Bind configuration is not bug-for-bug compatible with bind yet.

# Version 2.0 Release Candidate 1
The MacOS X release! A very experimental OS X 10.2 build has been added. Furthermore, the Windows version is now in line with Unix with respect to capabilities. The ODBC backend now has the code to function as both a master and a slave.

Developers: this version is compatible with 1.99.11 backends.

-   Implemented native packet response parsing code, allowing Windows to perform AXFR and NS and SOA queries.
-   This is the first version for which we have added support for Darwin 6.0, which is part of the forthcoming Mac OS X 10.2. Please note that although this version is marked RC1, that we have not done extensive testing yet. Consider this a technology preview.
    -   The Darwin version has been developed on Mac OS X 10.2 (6C35). Other versions may or may not work.
    -   Currently only the random, bind, mysql and pdns backends are included.
    -   The menu based installer script does not work, you will have to edit pathconfig by hand as outlined in chapter 2.
    -   On Mac OS X Client, PowerDNS will fail to start because a system service is already bound to port 53.

    This version is distributed as a compressed tar file. You should follow the generic UNIX installation instructions.

## Bugs fixed
-   Zone2sql PostgreSQL mode neglected to lowercase $ORIGIN. Thanks to Maikel Verheijen of Ladot for spotting this.
-   Zone2sql PostgreSQL mode neglected to remove a trailing dot from $ORIGIN if present. Thanks to Thanks to Maikel Verheijen of Ladot for spotting this.
-   Zone file parser was not compatible with bind when $INCLUDING non-absolute file names. Thanks to Jeff Miller for working out how this should work.
-   Bind configuration parser was not compatible with bind when including non-absolute file names. Thanks to Jeff Miller for working out how this should work.
-   Documentation incorrectly listed the Bind backend as 'slave capable'. This is not yet true, now labeled 'experimental'.

Windows changes. We are indebted to Dimitry Andric who educated us in the ways of distributing Windows software.

-   `pdns.conf` is now read if available.
-   Console version responds to ^c now.
-   Default pdns.conf added to distribution
-   Uninstaller missed several files, leaving remnants behind
-   DLLs are now installed locally, with the pdns executable.
-   pdns\_control is now also available on Windows
-   ODBC backend can now act as master and slave. Experimental.
-   The example zone missed indexes and had other faults.
-   A runtime DLL that is present on most windows systems (but not all!) was missing.

# Version 1.99.12 Prerelease
The Windows release! See [Installing on Microsoft Windows](authoritative/installation.md). Beware, windows support is still very fresh and untested. Feedback is very welcome.

Developers: this version is compatible with 1.99.11 backends.

-   Windows 2000 code base merge completed. This resulted in quite some changes on the Unix end of things, so this may impact reliability.
-   ODBC backend added for Windows. See [ODBC backend](authoritative/backend-deprecated.md#odbc-backend).
-   IBM DB2 Universal Database backend available for Linux. See [DB2 backend](authoritative/backend-deprecated.md#db2-backend "DB2 backend").
-   Zone2sql now understands $INCLUDE. Thanks to Amaze Internet for nagging about this
-   The SOA Minimum TTL now has a configurable default (**soa-minimum-ttl**)value to placate the DENIC requirements.
-   Added a limit on the simultaneous numbers of TCP connections to accept (**max-tcp-connections**). Defaults to 10.

## Bugs fixed
-   When operating in virtual hosting mode (See [Virtual hosting](authoritative/running.md#virtual-hosting "Virtual hosting")), the additional init.d scripts would not function correctly and interface with other pdns instances.
-   PowerDNS neglected to conserve case on answers. So a query for WwW.PoWeRdNs.CoM would get an answer listing the address of www.powerdns.com. While this did not confuse resolvers, it is better to conserve case. This has semantic consequences for all backends, which the documentation now spells out.
-   PostgreSQL backend was case sensitive and returned only answers in case an exact match was found. The Generic PostgreSQL backend is now officially all lower case and zone2sql in PostgreSQL mode enforces this. Documentation has been been updated to reflect the case change. Thanks to Maikel Verheijen of Ladot for spotting this!
-   Documentation bug - postgresql create/index statements created a duplicate index. If you've previously copy pasted the commands and not noticed the error, execute **CREATE INDEX rec\_name\_index ON records(name)** to remedy. Thanks to Jeff Miller for reporting this. This also lead to depressingly slow 'ANY' lookups for those of you doing benchmarks.

## Features
-   pdns\_control (see [pdns\_control](authoritative/running.md#pdnscontrol "pdns_control")) now opens the local end of its socket in `/tmp` instead of next to the remote socket (by default `/var/run`). This eases the way for allowing non-root access to pdns\_control. When running chrooted (see [Chapter 7, *Security settings & considerations*](common/security.md "Security settings & considerations")), the local socket again moves back to `/var/run`.
-   pdns\_control now has a 'version' command. See [Section 1.1, “pdns\_control”](authoritative/running.md#pdnscontrol "1.1. pdns_control").

# Version 1.99.11 Prerelease
This release is important because it is the first release which is accompanied by an Open Source Backend Development Kit, allowing external developers to write backends for PowerDNS. Furthermore, a few bugs have been fixed

-   Lines with only whitespace in zone files confused PowerDNS (thanks Henk Wevers)
-   PowerDNS did not properly parse TTLs with symbolic suffixes in zone files, ie 2H instead of 7200 (thanks Henk Wevers)

# Version 1.99.10 Prerelease
**IMPORTANT**: there has been a tiny license change involving free public webbased dns hosting, check out the changes before deploying!

PowerDNS is now feature complete, or very nearly so. Besides adding features, a lot of 'fleshing out' work is done now. There is an important performance bug fix which may have lead to disappointing benchmarks - so if you saw any of that, please try either this version or 1.99.8 which also does not have the bug.

This version has been very stable for us on multiple hosts, as was 1.99.9.

PostgreSQL users should be aware that while 1.99.10 works with the schema as presented in earlier versions, advanced features such as master or slave support will not work unless you create the new 'domains' table as well.

## Bugs fixed
-   Wildcard AAAA queries sometimes received an NXDOMAIN error where they should have gotten an empty NO ERROR. Thanks to Jeroen Massar for spotting this on the .TK TLD!
-   Do not disable the packetcache for 'recursion desired' packets unless a recursor was configured. Thanks to Greg Schueler for noticing this.
-   A failing backend would not be reinstated. Thanks to 'Webspider' for discovering this problem with PostgreSQL connections that die after prolonged inactivity.
-   Fixed loads of IPv6 transport problems. Thanks to Marco Davids and others for testing. Considered ready for production now.
-   **Zone2sql** printed a debugging statement on range $GENERATE commands. Thanks to Rene van Valkenburg for spotting this.

## Features
-   PowerDNS can now act as a master, sending out notifications in case of changes and allowing slaves to AXFR. Big rewording of replication support, domains are now either 'native', 'master' or 'slave'. See [Master/Slave operation & replication](authoritative/modes-of-operation.md "Master/Slave operation & replication") for lots of details.
-   **Zone2sql** in PostgreSQL mode now populates the 'domains' table for easy master, slave or native replication support.
-   Ability to run on IPv6 transport only
-   Logging can now happen under a 'facility' so all PowerDNS messages appear in their own file. See [Operational logging using syslog](common/logging.md "Operational logging using syslog").
-   Different OS releases of PowerDNS now get different install path defaults. Thanks to Mark Lastdrager for nagging about this and to Nero Imhard and Frederique Rijsdijk for suggesting saner defaults.
-   Infrastructure for 'also-notify' statements added.

# Version 1.99.9 Early Access Prerelease
This is again a feature and an infrastructure release. We are nearly feature complete and will soon start work on the backends to make sure that they are all master, slave and 'superslave' capable.

## Bugs fixed
-   PowerDNS sometimes sent out duplicate replies for packets passed to the recursing backend. Mostly a problem on SMP systems. Thanks to Mike Benoit for noticing this.
-   Out-of-bailiwick CNAMEs (ie, a CNAME to a domain not in PowerDNS) caused a 'ServFail' packet in 1.99.8, indicating failure, leading to hosts not resolving. Thanks to Martin Gillstrom for noticing this.
-   Zone2sql balked at zones edited under operating systems terminating files with ^Z (Windows). Thanks Brian Willcott for reporting this.
-   PostgreSQL backend logged the password used to connect. Now only does so in case of failure to connect. Thanks to 'Webspider' for noticing this.
-   Debian unstable distribution wrongly depended on home compiled PostgreSQL libraries. Thanks to Konrad Wojas for noticing this.

## Features
-   When operating as a slave, AAAA records are now supported in the zone. They were already supported in master zones.
-   IPv6 transport support - PowerDNS can now listen on an IPv6 socket using the **local-ipv6** setting.
-   Very silly randombackend added which appears in the documentation as a sample backend. See [Backend writers' guide](appendix/backend-writers-guide.md).
-   When transferring a slave zone from a master, out of zone data is now rejected. Malicious operators might try to insert bad records otherwise.
-   'Supermaster' support for automatic provisioning from masters. See [Supermaster automatic provisioning of slaves](authoritative/modes-of-operation.md#supermaster "Supermaster automatic provisioning of slaves").
-   Recursing backend can now live on a non-standard (!=53) port. See [Recursion](authoritative/recursion.md "Recursion").
-   Slave zone retrieval is now queued instead of immediate, which scales better and is more resilient to temporary failures.
-   **max-queue-length** parameter. If this many packets are queued for database attention, consider the situation hopeless and respawn.

## Internal
-   SOA records are now 'special' and each backend can optionally generate them in special ways. PostgreSQL backend does so when operating as a slave.
-   Writing backends is now a lot easier. See [Backend writers' guide](appendix/backend-writers-guide.md "Backend writers' guide").
-   Added Bindbackend to internal regression tests, confirming that it is compliant.

# Version 1.99.8 Early Access Prerelease
A lot of infrastructure work gearing up to 2.0. Some stability bugs fixed and a lot of new features.

## Bugs fixed
-   Bindbackend was overly complex and crashed on some systems on startup. Simplified launch code.
-   SOA fields were not always properly filled in, causing default values to go out on the wire
-   Obscure bug triggered by malicious packets (we know who you are) in SOA finding code fixed.
-   Magic serial number calculation contained a double free leading to instability.
-   Standards violation, questions for domains for which PowerDNS was unauthoritative now get a SERVFAIL answer. Thanks to the IETF Namedroppers list for helping out with this.
-   Slowly launching backends were being relaunched at a great rate when queries were coming in while launching backends.
-   MySQL-on-unix-domain-socket on SMP systems was overwhelmed by the quick connection rate on launch, inserted a small 50ms delay.
-   Some SMP problems appear to be compiler related. Shifted to GCC 3.0.4 for Linux.
-   Ran ispell on documentation.

## Feature enhancements
-   Recursing backend. See [Recursion](authoritative/recursion.md "Recursion"). Allows recursive and authoritative DNS on the same IP address.
-   [NAPTR support](types.md#naptr), which is especially useful for the ENUM/E.164 community.
-   Zone transfers can now be allowed per [netmask instead of only per IP address](authoritative/settings.md#allow-axfr-ips).
-   Preliminary support for slave operation included. Only for the adventurous right now! See [Slave operation](authoritative/modes-of-operation.md "Slave operation")
-   All record types now documented, see [Supported record types and their storage](types.md "Supported record types and their storage").

## Known bugs
- Wildcard CNAMEs do not work as they do with bind.
- Recursion sometimes sends out duplicate packets (fixed in 1.99.9 snapshots)
- Some stability issues which are caught by the guardian

## Missing features
Features present in this document, but disabled or withheld from the current release
-   gmysqlbackend, oraclebackend

# Version 1.99.7 Early Access Prerelease
Named.conf parsing got a lot of work and many more bind configurations can now be parsed. Furthermore, error reporting was improved. Stability is looking good.

## Bugs fixed
-   Bind parser got confused by file names with underscores and colons.
-   Bind parser got confused by spaces in quoted names
-   FreeBSD version now stops and starts when instructed to do so.
-   Wildcards were off by default, which violates standards. Now on by default.
-   --oracle was broken in zone2sql

## Feature enhancements
-   Line number counting goes on as it should when including files in named.conf
-   Added --no-config to enable users to start the pdns daemon without parsing the configuration file.
-   zone2sql now has --bare for unformatted output which can be used to generate insert statements for different database layouts
-   zone2sql now has --gpgsql, which is an alias for --mysql, to output in a format useful for the default Generic PostgreSQL backend
-   zone2sql is now documented.

## Known bugs
Wildcard CNAMEs do not work as they do with bind.

## Missing features
Features present in this document, but disabled or withheld from the current release
-   gmysqlbackend, oraclebackend

Some of these features will be present in newer releases.

# Version 1.99.6 Early Access Prerelease

This version is now running on dns-eu1.powerdns.net and working very well for us. But please remain cautious before deploying!

## Bugs fixed
-   Webserver neglected to show log messages
-   TCP question/answer miscounted multiple questions over one socket. Fixed misnaming of counter
-   Packetcache now detects clock skew and times out entries
-   named.conf parser now reports errors with line number and offending token
-   File names in named.conf can now contain:

## Feature enhancements
-   The webserver now by default does not print out configuration statements, which might contain database backends. Use **webserver-print-arguments** to restore the old behaviour.
-   Generic PostgreSQL backend is now included. Still rather beta.

## Known bugs
- FreeBSD version does not stop when requested to do so.
- Wildcard CNAMEs do not work as they do with bind.

## Missing features
Features present in this document, but disabled or withheld from the current release
-   gmysqlbackend, oraclebackend

Some of these features will be present in newer releases.

# Version 1.99.5 Early Access Prerelease
The main focus of this release is stability and TCP improvements. This is the first release PowerDNS-the-company actually considers for running on its production servers!

## Major bugs fixed
-   Zone2sql received a floating point division by zero error on named.confs with less than 100 domains.
-   Huffman encoder failed without specific error on illegal characters in a domain
-   Fixed huge memory leaks in TCP code.
-   Removed further file descriptor leaks in guardian respawning code
-   Pipebackend was too chatty.
-   pdns\_server neglected to close fds 0, 1 & 2 when daemonizing

## Feature enhancements
-   bindbackend can be instructed not to check the ctime of a zone by specifying **bind-check-interval=0**, which is also the new default.
-   **pdns\_server --list-modules** lists all available modules.

## Performance enhancements
-   TCP code now only creates a new database connection for AXFR.
-   TCP connections timeout rather quickly now, leading to less load on the server.

## Known bugs
- FreeBSD version does not stop when requested to do so.
- Wildcard CNAMEs do not work as they do with bind.

## Missing features
Features present in this document, but disabled or withheld from the current release
-   gmysqlbackend, oraclebackend, gpgsqlbackend

Some of these features will be present in newer releases.

# Version 1.99.4 Early Access Prerelease
A lot of new named.confs can now be parsed, zone2sql & bindbackend have gained features and stability.

## Major bugs fixed
-   Label compression was not always enabled, leading to large reply packets sometimes.
-   Database errors on TCP server lead to a nameserver reload by the guardian.
-   MySQL backend neglected to close its connection properly.
-   BindParser miss parsed some IP addresses and netmasks.
-   Truncated answers were also truncated on the packetcache, leading to truncated TCP answers.

## Feature enhancements
-   Zone2sql and the bindbackend now understand the Bind $GENERATE{} syntax.
-   Zone2sql can optionally gloss over non-existing zones with **--on-error-resume-next**.
-   Zone2sql and the bindbackend now properly expand @ also on the right hand side of records.
-   Zone2sql now sets a default TTL.
-   DNS UPDATEs and NOTIFYs are now logged properly and sent the right responses.

## Performance enhancements
-   'Fancy records' are no longer queried for on ANY queries - this is a big speedup.

## Known bugs
- FreeBSD version does not stop when requested to do so.
- Zone2sql refuses named.confs with less than 100 domains.
- Wildcard CNAMEs do not work as they do with bind.

## Missing features
Features present in this document, but disabled or withheld from the current release
-   gmysqlbackend, oraclebackend, gpgsqlbackend

Some of these features will be present in newer releases.

# Version 1.99.3 Early Access Prerelease
The big news in this release is the BindBackend which is now capable of parsing many more named.conf Bind configurations. Furthermore, PowerDNS has successfully parsed very large named.confs with large numbers of small domains, as well as small numbers of large domains (TLD).

Zone transfers are now also much improved.

Major bugs fixed
-   zone2sql leaked file descriptors on each domain, used wrong Bison recursion leading to parser stack overflows. This limited the amount of domains that could be parsed to 1024.
-   zone2sql can now read all known zone files, with the exception of those containing $GENERATE
-   Guardian relaunching a child lost two file descriptors
-   Don't die on a connection reset by peer during zone transfer.
-   Webserver does not crash anymore on ringbuffer resize

## Feature enhancements
-   AXFR can now be disabled, and re-enabled per IP address
-   --help accepts a parameter, will then show only help items with that prefix.
-   zone2sql now accepts a --zone-name parameter
-   BindBackend maturing - 9500 zones parsed in 3.5 seconds. No longer case sensitive.

## Performance enhancements
-   Implemented RFC-breaking AXFR format (which is the industry standard). Zone transfers now zoom along at wire speed (many megabits/s).

## Known bugs
- FreeBSD version does not stop when requested to do so.
- BindBackend cannot parse zones with $GENERATE statements.

## Missing features
Features present in this document, but disabled or withheld from the current release

-   gmysqlbackend, oraclebackend, gpgsqlbackend

Some of these features will be present in newer releases.

# Version 1.99.2 Early Access Prerelease

## Major bugs fixed
-   Database backend reload does not hang the daemon anymore
-   Buffer overrun in local socket address initialisation may have caused binding problems
-   setuid changed the uid to the gid of the selected user
-   zone2sql doesn't crash (dump core) on invocation anymore. Fixed lots of small issues.
-   Don't parse configuration file when creating configuration file. This was a problem with reinstalling.

## Performance improvements
-   removed a lot of unnecessary gettimeofday calls
-   removed needless select(2) call in case of listening on only one address
-   removed 3 useless syscalls in the fast path

Having said that, more work may need to be done. Testing on a 486 saw packet rates in a simple setup (question/wait/answer/question..) improve from 200 queries/second to over 400.

## Usability improvements
-   Fixed error checking in init.d script (**show**, **mrtg**)
-   Added 'uptime' to the mrtg output
-   removed further GNUisms from installer and init.d scripts for use on FreeBSD
-   Debian package and apt repository, thanks to Wichert Akkerman.
-   FreeBSD /usr/ports, thanks to Peter van Dijk (in progress).

Stability may be an issue as well as performance. This version has a tendency to log a bit too much which slows the nameserver down a lot.

## Known bugs
- Decreasing a ringbuffer on the website is a sure way to crash the daemon. Zone2sql, while improved, still has problems with a zone in the following format

```
name         IN            A        192.0.2.4
             IN            A        192.0.2.5
```

To fix, add 'name' to the second line.

Zone2sql does not close file descriptors.

FreeBSD version does not stop when requested via the init.d script.

## Missing features
Features present in this document, but disabled or withheld from the current release
-   gmysqlbackend, oraclebackend, gpgsqlbackend
-   fully functioning bindbackend - will try to parse named.conf, but probably fail

Some of these features will be present in newer releases.

# Version 1.99.1 Early Access Prerelease
This is the first public release of what is going to become PowerDNS 2.0. As such, it is not of production quality. Even PowerDNS-the-company does not run this yet.

Stability may be an issue as well as performance. This version has a tendency to log a bit too much which slows the nameserver down a lot.

## Known bugs
Decreasing a ringbuffer on the website is a sure way to crash the daemon. Zone2sql is very buggy.

## Missing features
Features present in this document, but disabled or withheld from the current release:

-   gmysqlbackend, oraclebackend, gpgsqlbackend
-   fully functioning bindbackend - will not parse configuration files

Some of these features will be present in newer releases.
