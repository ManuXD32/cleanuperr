# cleanuperr

## How it works

1. Add excluded file names to prevent malicious files from being downloaded by qBittorrent.
2. cleanuperr goes through all items in Sonarr's queue at every 5th minute.
3. For each queue item, a call is made to qBittorrent to get the stats of the torrent.
4. If a torrent is found to be marked as completed, but with 0 downloaded bytes, cleanuperr calls Sonarr to add that torrent to the blocklist.
5. If any malicious torrents have been found, cleanuperr calls Sonarr to automatically search again.

## Usage

### Docker
```
docker run -d \
    -e TRIGGERS__QUEUECLEANER="0 0/5 * * * ?" \
    -e QBITTORRENT__URL="http://localhost:8080" \
    -e QBITTORRENT__USERNAME="user" \
    -e QBITTORRENT__PASSWORD="pass" \
    -e SONARR__ENABLED=true \
    -e SONARR__INSTANCES__0__URL="http://localhost:8989" \
    -e SONARR__INSTANCES__0__APIKEY="secret1" \
    -e SONARR__INSTANCES__1__URL="http://localhost:8990" \
    -e SONARR__INSTANCES__1__APIKEY="secret2" \
    -e RADARR__ENABLED=true \
    -e RADARR__INSTANCES__0__URL="http://localhost:7878" \
    -e RADARR__INSTANCES__0__APIKEY="secret3" \
    -e RADARR__INSTANCES__1__URL="http://localhost:7879" \
    -e RADARR__INSTANCES__1__APIKEY="secret4" \
    ...
    flaminel/cleanuperr:latest
```

### Docker compose yaml
```
version: "3.3"
services:
  cleanuperr:
    environment:
      - TRIGGERS__QUEUECLEANER=0 0/5 * * * ?
      - QBITTORRENT__URL=http://localhost:8080
      - QBITTORRENT__USERNAME=user
      - QBITTORRENT__PASSWORD=pass
      - SONARR__ENABLED=true
      - SONARR__INSTANCES__0__URL=http://localhost:8989
      - SONARR__INSTANCES__0__APIKEY=secret1
      - SONARR__INSTANCES__1__URL=http://localhost:8990
      - SONARR__INSTANCES__1__APIKEY=secret2
      - RADARR__ENABLED=true
      - RADARR__INSTANCES__0__URL=http://localhost:7878
      - RADARR__INSTANCES__0__APIKEY=secret3
      - RADARR__INSTANCES__1__URL=http://localhost:7879
      - RADARR__INSTANCES__1__APIKEY=secret4
    image: flaminel/cleanuperr:latest
    restart: unless-stopped
```

### Environment variables

| Variable | Required | Description | Default value |
|---|---|---|---|
| TRIGGERS__QUEUECLEANER | No | [Quartz cron trigger](https://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) | 0 0/5 * * * ? |
| QBITTORRENT__URL | Yes | qBittorrent instance url | http://localhost:8080 |
| QBITTORRENT__USERNAME | Yes | qBittorrent user | empty |
| QBITTORRENT__PASSWORD | Yes | qBittorrent password | empty |
|||||
| SONARR__ENABLED | No | Whether Sonarr cleanup is enabled or not  | true |
| SONARR__INSTANCES__0__URL | Yes | First Sonarr instance url | http://localhost:8989 |
| SONARR__INSTANCES__0__APIKEY | Yes | First Sonarr instance API key | empty |
|||||
| RADARR__ENABLED | No | Whether Radarr cleanup is enabled or not  | false |
| RADARR__INSTANCES__0__URL | Yes | First Radarr instance url | http://localhost:8989 |
| RADARR__INSTANCES__0__APIKEY | Yes | First Radarr instance API key | empty |

#

Multiple Sonarr/Radarr instances can be specified using this format:

```
SONARR__INSTANCES__<NUMBER>__URL
SONARR__INSTANCES__<NUMBER>__APIKEY
```

where `<NUMBER>` starts from 0.

#

### Binaries
1. Download the binaries from [releases](https://github.com/flmorg/cleanuperr/releases).
2. Extract them from the zip file.
3. Edit **appsettings.json**. The paths from this json file correspond with the docker env vars, as described [above](/README.md#environment-variables).
## Extensions to block in qBittorrent
<details> 
    <summary>Extensions</summary>
    <pre><code>*(sample).*
*.0xe
*.73k
*.73p
*.7z
*.89k
*.89z
*.8ck
*.a7r
*.ac
*.acc
*.ace
*.acr
*.actc
*.action
*.actm
*.ade
*.adp
*.afmacro
*.afmacros
*.ahk
*.ai
*.aif
*.air
*.alz
*.api
*.apk
*.app
*.appimage
*.applescript
*.application
*.appx
*.arc
*.arj
*.arscript
*.asb
*.asp
*.aspx
*.aspx-exe
*.atmx
*.azw2
*.ba_
*.bak
*.bas
*.bash
*.bat
*.bdjo
*.bdmv
*.beam
*.bin
*.bmp
*.bms
*.bns
*.bsa
*.btm
*.bz2
*.c
*.cab
*.caction
*.cci
*.cda
*.cdb
*.cel
*.celx
*.cfs
*.cgi
*.cheat
*.chm
*.ckpt
*.cla
*.class
*.clpi
*.cmd
*.cof
*.coffee
*.com
*.command
*.conf
*.config
*.cpl
*.crt
*.cs
*.csh
*.csharp
*.csproj
*.css
*.csv
*.cue
*.cur
*.cyw
*.daemon
*.dat
*.data-00000-of-00001
*.db
*.deamon
*.deb
*.dek
*.diz
*.dld
*.dll
*.dmc
*.dmg
*.doc
*.docb
*.docm
*.docx
*.dot
*.dotb
*.dotm
*.drv
*.ds
*.dw
*.dword
*.dxl
*.e_e
*.ear
*.ebacmd
*.ebm
*.ebs
*.ebs2
*.ecf
*.eham
*.elf
*.elf-so
*.email
*.emu
*.epk
*.es
*.esh
*.etc
*.ex4
*.ex5
*.ex_
*.exe
*.exe-only
*.exe-service
*.exe-small
*.exe1
*.exopc
*.exz
*.ezs
*.ezt
*.fas
*.fba
*.fky
*.flac
*.flatpak
*.flv
*.fpi
*.frs
*.fxp
*.gadget
*.gat
*.gif
*.gifv
*.gm9
*.gpe
*.gpu
*.gs
*.gz
*.h5
*.ham
*.hex
*.hlp
*.hms
*.hpf
*.hta
*.hta-psh
*.htaccess
*.htm
*.html
*.icd
*.icns
*.ico
*.idx
*.iim
*.img
*.index
*.inf
*.ini
*.ink
*.ins
*.ipa
*.ipf
*.ipk
*.ipsw
*.iqylink
*.iso
*.isp
*.isu
*.ita
*.izh
*.izma ace
*.jar
*.java
*.jpeg
*.jpg
*.js
*.js_be
*.js_le
*.jse
*.jsf
*.json
*.jsp
*.jsx
*.kix
*.ksh
*.kx
*.lck
*.ldb
*.lib
*.link
*.lnk
*.lo
*.lock
*.log
*.loop-vbs
*.ls
*.m3u
*.m4a
*.mac
*.macho
*.mamc
*.manifest
*.mcr
*.md
*.mda
*.mdb
*.mde
*.mdf
*.mdn
*.mdt
*.mel
*.mem
*.meta
*.mgm
*.mhm
*.mht
*.mhtml
*.mid
*.mio
*.mlappinstall
*.mlx
*.mm
*.mobileconfig
*.model
*.moo
*.mp3
*.mpa
*.mpk
*.mpls
*.mrc
*.mrp
*.ms
*.msc
*.msh
*.msh1
*.msh1xml
*.msh2
*.msh2xml
*.mshxml
*.msi
*.msi-nouac
*.msix
*.msl
*.msp
*.mst
*.msu
*.mxe
*.n
*.ncl
*.net
*.nexe
*.nfo
*.nrg
*.num
*.nzb.bz2
*.nzb.gz
*.nzbs
*.ocx
*.odt
*.ore
*.ost
*.osx
*.osx-app
*.otm
*.out
*.ova
*.p
*.paf
*.pak
*.pb
*.pcd
*.pdb
*.pdf
*.pea
*.perl
*.pex
*.phar
*.php
*.php5
*.pif
*.pkg
*.pl
*.plsc
*.plx
*.png
*.pol
*.pot
*.potm
*.powershell
*.ppam
*.ppkg
*.pps
*.ppsm
*.ppt
*.pptm
*.pptx
*.prc
*.prg
*.ps
*.ps1
*.ps1xml
*.ps2
*.ps2xml
*.psc1
*.psc2
*.psd
*.psd1
*.psh
*.psh-cmd
*.psh-net
*.psh-reflection
*.psm1
*.pst
*.pt
*.pvd
*.pwc
*.pxo
*.py
*.pyc
*.pyd
*.pyo
*.python
*.pyz
*.qit
*.qpx
*.ram
*.rar
*.raw
*.rb
*.rbf
*.rbx
*.readme
*.reg
*.resources
*.resx
*.rfs
*.rfu
*.rgs
*.rm
*.rox
*.rpg
*.rpj
*.rpm
*.ruby
*.run
*.rxe
*.s2a
*.sample
*.sapk
*.savedmodel
*.sbs
*.sca
*.scar
*.scb
*.scf
*.scpt
*.scptd
*.scr
*.script
*.sct
*.seed
*.server
*.service
*.sfv
*.sh
*.shb
*.shell
*.shortcut
*.shs
*.shtml
*.sit
*.sitx
*.sk
*.sldm
*.sln
*.smm
*.snap
*.snd
*.spr
*.sql
*.sqx
*.srec
*.srt
*.ssm
*.sts
*.sub
*.svg
*.swf
*.sys
*.tar
*.tar.gz
*.tbl
*.tbz
*.tcp
*.text
*.tf
*.tgz
*.thm
*.thmx
*.thumb
*.tiapp
*.tif
*.tiff
*.tipa
*.tmp
*.tms
*.toast
*.torrent
*.tpk
*.txt
*.u3p
*.udf
*.upk
*.upx
*.url
*.uvm
*.uw8
*.vb
*.vba
*.vba-exe
*.vba-psh
*.vbapplication
*.vbe
*.vbs
*.vbscript
*.vbscript 
*.vcd
*.vdo
*.vexe
*.vhd
*.vhdx
*.vlx
*.vm
*.vmdk
*.vob
*.vocab
*.vpm
*.vxp
*.war
*.wav
*.wbk
*.wcm
*.webm
*.widget
*.wim
*.wiz
*.wma
*.workflow
*.wpk
*.wpl
*.wpm
*.wps
*.ws
*.wsc
*.wsf
*.wsh
*.x86
*.x86_64
*.xaml
*.xap
*.xbap
*.xbe
*.xex
*.xig
*.xla
*.xlam
*.xll
*.xlm
*.xls
*.xlsb
*.xlsm
*.xlsx
*.xlt
*.xltb
*.xltm
*.xlw
*.xml
*.xqt
*.xrt
*.xys
*.xz
*.ygh
*.z
*.zip
*.zipx
*.zl9
*.zoo
*sample.avchd
*sample.avi
*sample.mkv
*sample.mov
*sample.mp4
*sample.webm
*sample.wmv
Trailer.*
VOSTFR
api
</code></pre>
</details>
