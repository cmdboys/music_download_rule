
## config
size_list = ['size_128mp3', 'size_320mp3', 'size_ape', 'size_flac']


## 1. 使用一首傀儡获取getKey
```py
def get_key():
    uin = '1008611'
    guid = '1234567890'
    getVkeyUrl = 'https://c.y.qq.com/base/fcgi-bin/fcg_music_express_mobile3.fcg?g_tk=0&loginUin={uin}&hostUin=0&format=json&inCharset=utf8&outCharset=utf-8&notice=0&platform=yqq&needNewCode=0&cid=205361747&uin={uin}&songmid=003a1tne1nSz1Y&filename=C400003a1tne1nSz1Y.m4a&guid={guid}'
    url = getVkeyUrl.format(uin=uin, guid=guid)
    try:
        r = requests.get(url, headers=headers)
        json = r.json()
        vkey = json['data']['items'][0]['vkey']
        if len(vkey) == 112:
            return vkey
    except:
        pass
    return None
}
```
```json
{
	"code": 0,
	"cid": 205361747,
	"userip": "123.14.78.103",
	"data": {
		"expiration": 80400,
		"items": [{
			"subcode": 0,
			"songmid": "003a1tne1nSz1Y",
			"filename": "C400003a1tne1nSz1Y.m4a",
			"vkey": "DD0612F382532A8CC24D0D0C5A554CB04868E3B21FA7D81B55D9F1D9E6DF348A852B6BB992BC1D0B2B78DF131316FEDFFD94E636154ACB40"
		}]
	}
}
```

## 2. 获取info
mid 001peMVw1OPkB6
info_url = 'https://c.y.qq.com/v8/fcg-bin/fcg_play_single_song.fcg?songmid={songmid}&tpl=yqq_song_detail&format=json&callback=getOneSongInfoCallback&g_tk=5381&jsonCallback=getOneSongInfoCallback&loginUin=0&hostUin=0&format=json&inCharset=utf8&outCharset=utf-8&notice=0&platform=yqq&needNewCode=0'

```json
{
	"code": 0,
	"data": [{
		"action": {
			"alert": 2,
			"icons": 139132,
			"msgdown": 0,
			"msgfav": 0,
			"msgid": 14,
			"msgpay": 6,
			"msgshare": 0,
			"switch": 17409795
		},
		"album": {
			"id": 5789354,
			"mid": "002dJsSq1pMtpy",
			"name": "中国梦之声·下一站传奇 第12期",
			"subtitle": "",
			"time_public": "2019-01-06",
			"title": "中国梦之声·下一站传奇 第12期"
		},
		"bpm": 0,
		"data_type": 0,
		"file": {
			"b_30s": 0,
			"e_30s": 0,
			"hires_bitdepth": 0,
			"hires_sample": 0,
			"media_mid": "003IdfIV0HBkbj",
			"size_128mp3": 3283295,
			"size_192aac": 4966946,
			"size_192ogg": 4825824,
			"size_24aac": 637811,
			"size_320mp3": 8207899,
			"size_48aac": 1251643,
			"size_96aac": 2505367,
			"size_ape": 0,
			"size_dts": 0,
			"size_flac": 27540694,
			"size_hires": 0,
			"size_try": 0,
			"try_begin": 0,
			"try_end": 0,
			"url": ""
		},
		"fnote": 4009,
		"genre": 1,
		"id": 226989122,
		"index_album": 1,
		"index_cd": 0,
		"interval": 205,
		"isonly": 1,
		"ksong": {
			"id": 0,
			"mid": ""
		},
		"label": "0",
		"language": 0,
		"mid": "001peMVw1OPkB6",
		"modify_stamp": 0,
		"mv": {
			"id": 0,
			"name": "",
			"title": "",
			"vid": ""
		},
		"name": "英雄上线",
		"pay": {
			"pay_down": 1,
			"pay_month": 1,
			"pay_play": 0,
			"pay_status": 0,
			"price_album": 0,
			"price_track": 200,
			"time_free": 0
		},
		"singer": [{
			"id": 2688800,
			"mid": "003oMGiN0I0Nvl",
			"name": "挺齐全战队",
			"title": "挺齐全战队",
			"type": 2,
			"uin": 0
		}],
		"status": 0,
		"subtitle": "",
		"time_public": "2019-01-06",
		"title": "英雄上线 (Live)",
		"trace": "",
		"type": 0,
		"url": "http://stream4.qqmusic.qq.com/238989122.wma",
		"version": 3,
		"volume": {
			"gain": -6.9740,
			"lra": 4.7310,
			"peak": 0.9690
		}
	}],
	"url": {
		"226989122": "ws.stream.qqmusic.qq.com/C100001peMVw1OPkB6.m4a?fromtag=38"
	},
	"url1": {},
	"extra_data": [],
	"joox": 0,
	"joox_login": 0,
	"msgid": 0
}
```

获取到 media id = 003IdfIV0HBkbj


## 3. 获取下载链接 get_link

```py
type_info = [['M500', 'mp3'], ['M800', 'mp3'], ["A000", 'ape'], ['F000', 'flac']]


dl_url = 'http://streamoc.music.tc.qq.com/{prefix}{media_mid}.{type}?vkey={vkey}&guid=1234567890&uin=1008611&fromtag=8'


def get_link(media_mid, vkey):
    type_info = [['M500', 'mp3'], ['M800', 'mp3'], ["A000", 'ape'], ['F000', 'flac']]
    dl_url = 'http://streamoc.music.tc.qq.com/{prefix}{media_mid}.{type}?vkey={vkey}&guid=1234567890&uin=1008611&fromtag=8'
    dl = []
    for item in type_info:
        s_url = dl_url.format(prefix=item[0], media_mid=media_mid, type=item[1], vkey=vkey)
        dl.append(s_url)
    return dl
```    
