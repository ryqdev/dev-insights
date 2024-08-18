
```shell
(base) ➜  ~ neofetch
       _,met$$$$$gg.          xxxxxxxx@xxxxxxxxxxxxx
    ,g$$$$$$$$$$$$$$$P.       --------------- 
  ,g$$P"     """Y$$.".        OS: Debian GNU/Linux 12 (bookworm) x86_64 
 ,$$P'              `$$$.     Host: XPS 15 9530 
',$$P       ,ggs.     `$$b:   Kernel: 6.1.0-22-amd64 
`d$$'     ,$P"'   .    $$$    Uptime: 59 mins 
 $$P      d$'     ,    $$P    Packages: 2604 (dpkg) 
 $$:      $$.   -    ,d$$'    Shell: zsh 5.9 
 $$;      Y$b._   _,d$P'      Resolution: 3840x2160 
 Y$$.    `.`"Y$$$$P"'         DE: GNOME 43.9 
 `$$b      "-.__              WM: Mutter 
  `Y$$                        WM Theme: Adwaita 
   `Y$$.                      Theme: Adwaita [GTK2/3] 
     `$$b.                    Icons: Adwaita [GTK2/3] 
       `Y$$b.                 Terminal: vscode 
          `"Y$b._             CPU: 13th Gen Intel i7-13700H (20) @ 4.800GHz 
              `"""            GPU: NVIDIA GeForce RTX 4050 Max-Q / Mobile 
                              GPU: Intel Raptor Lake-P [Iris Xe Graphics] 
                              Memory: 11918MiB / 15661MiB 
```

## How to add fingerprint authentication in debian

```shell
sudo pam-auth-update
```

If there is not fingerprint authentication option, add it with 

```shell
sudo apt install fprintd libpam-fprintd
```

## How to install nvidia in debian

https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html
https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Debian&target_version=12&target_type=deb_local