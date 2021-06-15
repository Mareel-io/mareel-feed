# Mareel OpenWRT feed
## Description

OpenWRT feed for Mareel packages.

Currently support OpenWRT master(dev branch) only

Note: This feed require Node.JS from

https://github.com/nxhack/openwrt-node-packages 

to operate properly.

## License

See [LICENSE](LICENSE) file.

Lots of Makefiles are based on @nxhack's nodejs package makefile

## Usage

Add the following line to `feeds.conf`

```
src-git node https://github.com/nxhack/openwrt-node-packages.git
src-git mareel https://github.com/Mareel-io/mareel-feed.git
```

Run

```
./scripts/feeds update node
./scripts/feeds update mareel
rm ./package/feeds/packages/node
rm ./package/feeds/packages/node-*
./scripts/feeds install -a -p node
./scripts/feeds install -a -p mareel
```