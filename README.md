# WebAssembly System Interface implementation for Deno

## Example

```typescript
import WASI from "https://deno.land/x/wasi/mod.ts";

const wasi = new WASI({
	args: Deno.args,
	env: Deno.env,
});

const binary = Deno.readAll("command.wasm");
const module = await WebAssembly.compile(binary);
const instance = await WebAssembly.instantiate(module, {
	wasi_snapshot_preview1: wasi.exports,
});

wasi.memory = module.exports.memory;

if (module.exports._start) {
	instance.exports._start();
} else if (module.exports._initialize) {
	instance.exports._initialize();
} else {
	throw new Error("No entry point found");
}
```
