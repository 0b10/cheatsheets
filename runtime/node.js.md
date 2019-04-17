# TOC
---

* [Require](#require)

# Require <a name="require"></a>
---

### Caching

* Modules are cached based on their resolved file name.
* The produced object is a singleton, one object per unique file.
* `require('foo')` in two separate modules, will result in the exact same exported instance (if both statements resolve to the same file).

### Resolution
File resolution and case sensitivity depends on the underlying filesystem. For case-insensitive filesystems, 'FOO' and 'foo' could very well resolve to the same file.

<details>
	<summary>Resources</summary>
	[Modules are Singletons](https://medium.com/@lazlojuly/are-node-js-modules-singletons-764ae97519af) | [Official Docs](https://nodejs.org/api/modules.html#modules_require_id)
</details>

---