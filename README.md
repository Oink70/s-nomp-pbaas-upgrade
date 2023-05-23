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
 1) shutdown pool and pm2 (`pm2 kill`)
 2) stop verus daemon(s)

### Verus changes:
 3) stop verus daemon
 4) add `miningdistribution='{"<FEE-ADDRESS>":5,"<MINING-ADDRESS>":95}'`, `mint=1`, `gen=1` and `genproclimit=0` to your `VRSC.conf`. (fill in your `<FEE-ADDRESS>`, `<MINING-ADDRESS>` and own percentages). If your fee address is **not** in the local wallet (advisable), you also need `minetolocalwallet=0`.
 5) edit the VRSC.conf file and change the `blocknotify=` line to contain the right node version (`v10.24.1`)
 6) Start verus daemon
 
### Nodejs upgrade:
 7) check your node version with `echo $(nvm current)`.
 8) update nodejs to version 12: `nvm install 10.24.1 --reinstall-packages-from=$(nvm current)`
 9) set node version 10 as default: `nvm alias default 10`
10) exit and re-enter your pool user: `exit` and as root `su - pool`.
11) remove old node version: `nvm uninstall 8.17.0` (adjust for the node version you got in step 5 if needed)
12) confirm with `nvm ls` that you are indeed using node version 10
13) check where your node is with `which node`
14) change to the directory you got above (the final `..../node` is the binary, so you need to leave that off)
15) create a symlink for nodejs: `ln -s node nodejs`
 
### S-Nomp upgrade:
16) `su - pool`
17) `cd s-nomp`
18) `wget https://github.com/Oink70/s-nomp-pbaas-upgrade/releases/download/v0.0.1/PBaaS-upgrade.tar.gz`
19) `tar -xvf PBaaS-upgrade.tar.gz`
20) `npm rebuild --update-binary`

And start the pool.

In case your pool doesn't accept miner connections, please restart the pool with `pm2 restart all -a`.

