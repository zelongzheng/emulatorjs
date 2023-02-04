# emulatorjs
source  by www.emulatorjs.com

# warning 请常备份核心 以及本地化流程(Localization process)
> the core some time will update.maybe save the cores to you local computer,backup!
```javascript
    //f12 console
    let CoreSystemList = {
        'psx': 'psx',
        'nds': 'nds',
        'nes': 'nes',
        'mesen': 'mesen',
        'snes': 'snes',
        'snes2002': 'snes2002',
        'snes2005': 'snes2005',
        'snes2010': 'snes2010',
        'gb': 'gb',
        'gbc': 'gb',
        'gba': 'gba',
        'vbanext': 'vbanext',
        'vb': 'vb',
        'n64': 'n64',
        '3do': '3do',
        'segaSaturn': 'saturn',
        'ngp': 'ngp',
        'pce': 'pce',
        'pcfx': 'pcfx',
        'msx': 'bluemsx',
        'atari2600': 'a2600',
        'atari7800': 'a7800',
        'lynx': 'lynx',
        'ws': 'ws',
        'arcade': 'arcade',
        //'fba0.2.97.29': 'fba0.2.97.29',
        'fbneo': 'fbneo',
        'jaguar': 'jaguar',
        'mame2003': 'mame2003',
        'mame0.193': 'mame',
        //'mame0.243': 'mame0.243',
        'sega': 'sega',
        'segaMS': 'sega',
        'segaGG': 'sega',
        'segaMD': 'sega',
        'segaCD': 'segacd',
        'sega32x': '32x',
        'zc210':'zc210',
        'openbor':'openbor',
        'n64-legacy':'n64-legacy',
        'amiga':'amiga',
        'px68k':'px68k',
        'beetle-psx':'beetle-psx',
        'mupen64plus':'mupen64plus',
        'mupen64plus2':'mupen64plus2',
        'mupen64plus-nx':'mupen64plus-nx',
        'vbanext':'vbanext',
        
        'mame-1': 'mame',
        'mame-2': 'mame',
        'mame-3': 'mame',
        'mame-4': 'mame',
        'mame-5': 'mame',
        'mame-6': 'mame',
        'mame-7': 'mame',
        
        'mame-1': 'mame0.243',
        'mame-2': 'mame0.243',
        'mame-3': 'mame0.243',
        'mame-4': 'mame0.243',
        'mame-5': 'mame0.243',
        'mame-6': 'mame0.243',
        'mame-7': 'mame0.243',
        'mame-8': 'mame0.243',



    };
    let jsonUrl = str=> `https://www.emulatorjs.com/api/v?name=${str}&_t=${Nenge.time}`;
    let CoreUrl = str=>`https://www.emulatorjs.com/cores/${str}?v=${Nenge.time}`;
    let func = [],kk={};
    Nenge.I.toArr(CoreSystemList,entry=>{
        func.push(async e=>{
            await Nenge.FetchItem({
                url:CoreUrl(entry[0]+'-wasm.data'),
                Filter(wasmU8){
                    let mime = Nenge.F.checkBuffer(wasmU8);
                    if (!mime || !['zip', '7z'].includes(mime)) {
                        wasmU8 = wasmU8['slice'](0xc);
                        wasmU8['set']([0x37, 0x7a, 0xbc, 0xaf, 0x27, 0x1c, 0x0, 0x3], 0x0);
                    }
                    return wasmU8;
                },
                success(data){
                    Nenge.down(entry[0]+'-wasm.7z',data)
                }
            });
            await Nenge.FetchItem({
                url:CoreUrl(entry[0]+'-asmjs.data'),
                Filter(wasmU8){
                    let mime = Nenge.F.checkBuffer(wasmU8);
                    if (!mime || !['zip', '7z'].includes(mime)) {
                        wasmU8 = wasmU8['slice'](0xc);
                        wasmU8['set']([0x37, 0x7a, 0xbc, 0xaf, 0x27, 0x1c, 0x0, 0x3], 0x0);
                    }
                    return wasmU8;
                },
                success(data){
                    Nenge.down(entry[0]+'-asmjs.7z',data)
                }
            });
            if(kk[entry[1]]) return;
            await Nenge.FetchItem({
                url:jsonUrl(entry[1]),
                type:'text',
                success(data){
                    Nenge.down(entry[1]+'.json',data)
                }
            });
            kk[entry[1]] = true;
        });
    });
    let xx;
    while(xx = func.pop()){
        await xx();
    }
```

# 如何使用自己的核心
```javascript
//search 搜索
var LoadWasmData = function (coreName, coreVersion) 
//at 在
url: `${EmulatorCoresHost}/cores/${coreName}?v=${coreVersion}`,
//add condition 增加判断即可
```

# 旧核心使用 旧核心在本仓库其他分支可下载.
> 旧核心并不需要读取动态json因此下载好对应json后.
```javascript
//search
T.FetchItem({
    url: jsonUrl,
    type: 'json',
    success(JsonData, headers) {
```