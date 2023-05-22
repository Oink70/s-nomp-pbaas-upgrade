# s-nomp-pbaas-upgrade
Temporary repo to host updated archive for quick S-NOMP updating.

## changed files for S-Nomp PBaaS:

1) verushash-node:
 - binding.gyp
 - verushash.cc
needs recompiling after updating files

2) node-stratum-pool
 - lib/blockTemplate.js
 - lib/jobManager.js
 - lib/pool.js
 - lib/stratum.js
 
3) s-nomp
 - coins/verustest.json
 - libs/poolWorker.js
 - libs/shareProcessor.js
 
## procedure (quick'n dirty):
 1) shutdown pool
 2) stop verus daemons

### Verus changes:
 3) stop verus daemon
 4) add `-miningdistribution='{"<FEE-ADDRESS>":5,"<MINING-ADDRESS>":95}' -mint -gen -genproclimit=0` to your VRSC daemon start command or add these entries in your `VRSC.conf`. (fill in your `<FEE-ADDRESS>`, `<MINING-ADDRESS>` and own percentages). If your fee address is **not** in the local wallet (advisable), you also need `-minetolocalwallet=0`
 5) Start verus daemon
 
### Nodejs upgrade:
 6) check your node version with `echo $(nvm current)`.
 7) update nodejs to version 12: `nvm install 10.24.1 --reinstall-packages-from=$(nvm current)`
 8) set node version 10 as default: `nvm alias default 10`
 9) exit and re-enter your pool user: `exit` and as root `su - pool`.
10) remove old node version: `nvm uninstall 8.17.0` (adjust for the node version you got in step 5 if needed)
11) confirm with `nvm ls` that you are indeed using node version 10
12) check where your node is with `which node`
13) change to the directory you got above (the final `..../node` is the binary, so you need to leave that off)
14) create a symlink for nodejs: `ln -s node nodejs`
 
### S-Nomp upgrade:
15) `su - pool`
16) `cd s-nomp`
17) `wget https://github.com/Oink70/s-nomp-pbaas-upgrade/releases/download/v0.0.1/PBaaS-upgrade.tar.gz`
18) `tar -xvf PBaaS-upgrade.tar.gz`
19) `npm rebuild --update-binary`

And start the pool.
