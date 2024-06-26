# CasinoGate Schema

## Casino Gate games
In that scenario, we build our own games and serve them to operators.


[![](https://mermaid.ink/img/pako:eNp9U8GO2jAQ_RXLp60EKGwITXLopVvRC1VVOFVcZuMhcUvs1B7vdgv8e21HWbKI1ifP88x7b8b2kVdaIC_5_qCfqwYMse3DTjG_rHusDXQNcxbNRgrs4bAC0keoxFW27tAA6RsVbDr9wE5SSWI1tMgsWiu1OlVgpdJ3Wh2kQtZH7y6lPRCLoa4N1oH9Sv1y8C8RJhWhd0g-uHL8MQqsgEZ-R9k9o_XcPRtGOvF4qzmL5Dr2bJk38nm7_coqrRRWkebJDmaYFCfvbqwXokBxauEnsoaoCxRga8kM_nJo6RT72aB5Gmb_pomVP7zAPeWQHr2NDd843mpnVIuKbABl9X-yb19Wd0GRrYEaNg3x6MZer2WoXIOCepgRiB_Okp8FkVS1vYzSl_EJb9G0IIV_kscA7zg16Fvjpd8K3IM70I7v1NmngiO9eVEVL8k4nHDXCT_GBwl-JC0v93CwHu1A8fLIf_MyT2dZmt0vi3S-TO-zJJ_wF14WsyQtsmKeLxZ5lhaL_Dzhf7T2BMksf5_M0zRbJkUyXxbZIPFJSP8cXhUwhuv-I8X_FFW_R5Jg7fwXo_cRbQ?type=png)](https://mermaid.live/edit#pako:eNp9U8GO2jAQ_RXLp60EKGwITXLopVvRC1VVOFVcZuMhcUvs1B7vdgv8e21HWbKI1ifP88x7b8b2kVdaIC_5_qCfqwYMse3DTjG_rHusDXQNcxbNRgrs4bAC0keoxFW27tAA6RsVbDr9wE5SSWI1tMgsWiu1OlVgpdJ3Wh2kQtZH7y6lPRCLoa4N1oH9Sv1y8C8RJhWhd0g-uHL8MQqsgEZ-R9k9o_XcPRtGOvF4qzmL5Dr2bJk38nm7_coqrRRWkebJDmaYFCfvbqwXokBxauEnsoaoCxRga8kM_nJo6RT72aB5Gmb_pomVP7zAPeWQHr2NDd843mpnVIuKbABl9X-yb19Wd0GRrYEaNg3x6MZer2WoXIOCepgRiB_Okp8FkVS1vYzSl_EJb9G0IIV_kscA7zg16Fvjpd8K3IM70I7v1NmngiO9eVEVL8k4nHDXCT_GBwl-JC0v93CwHu1A8fLIf_MyT2dZmt0vi3S-TO-zJJ_wF14WsyQtsmKeLxZ5lhaL_Dzhf7T2BMksf5_M0zRbJkUyXxbZIPFJSP8cXhUwhuv-I8X_FFW_R5Jg7fwXo_cRbQ)

Where:
- userSide: is a player within his browser
- operatorSide: is an online casino platform and could be an integrator gateway
- CasinoGate: actual infrastructure of CasinoGate
- integration: this is custom api for integration with ours client
- gameManager: is an admin panel, from which you can control game availability, math profile etc.
- game: this block encapsulates exact game backend services
	- gameServer
	- Game Math: module that we certify
	- db: just database
- gate: is a service that allows making requests from userSide to gameServer.

## External games
In this scenario, we distribute games provided by other game providers.

[![](https://mermaid.ink/img/pako:eNp1UsFO40AM_ZWRTyCVKiSBTqMVF7qq9tDVCjihXEzGTQc1M9XMBOiW_jtO0jZRl51T_Gy_9-x4B4VVBBks1_a9WKEL4mmWG8HP1y-lw81K1J7co1bUwc1rkC4io86q7YYcBvtNh_hxdXUnPrXRQZRYkfDkvbbms0Cvjb2wZq0NiS667Hs7oOvGsnRUNvxn-n3ivzJCm0BsMnBwZvq-lZhjGFgeVB8oPbN3dNTyqZe--kQ1Z8k_zn5s-1zzyiMsGqph52mEY9kCDZa8rlYT1WvtA4uGoE3pe9FTW8_cujyEb7x-3lF-Pukw_cssHfrg6iLUbjD5w--5-JfqIAojqMhVqBVfza6BcwgrqiiHjD8VLbFehxxys-dSrIN93JoCMpahEdQbxUueaWQ3FWRLXHtGN2gg28EHZNdRMp7G8nYqYyllksZyBFvI0sk4kRMZx9c3U06lcj-Cv9YyQzSepLfTNJokcRTF8Y1MjiI_leb_ddKgNlx0194efav73LI05vZfK_X3nA?type=png)](https://mermaid.live/edit#pako:eNp1UsFO40AM_ZWRTyCVKiSBTqMVF7qq9tDVCjihXEzGTQc1M9XMBOiW_jtO0jZRl51T_Gy_9-x4B4VVBBks1_a9WKEL4mmWG8HP1y-lw81K1J7co1bUwc1rkC4io86q7YYcBvtNh_hxdXUnPrXRQZRYkfDkvbbms0Cvjb2wZq0NiS667Hs7oOvGsnRUNvxn-n3ivzJCm0BsMnBwZvq-lZhjGFgeVB8oPbN3dNTyqZe--kQ1Z8k_zn5s-1zzyiMsGqph52mEY9kCDZa8rlYT1WvtA4uGoE3pe9FTW8_cujyEb7x-3lF-Pukw_cssHfrg6iLUbjD5w--5-JfqIAojqMhVqBVfza6BcwgrqiiHjD8VLbFehxxys-dSrIN93JoCMpahEdQbxUueaWQ3FWRLXHtGN2gg28EHZNdRMp7G8nYqYyllksZyBFvI0sk4kRMZx9c3U06lcj-Cv9YyQzSepLfTNJokcRTF8Y1MjiI_leb_ddKgNlx0194efav73LI05vZfK_X3nA)
Where:
gameProxy: is a service that wraps gameProvider api.
