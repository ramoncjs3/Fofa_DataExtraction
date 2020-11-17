# -*- coding: utf-8 -*-
# Author: Ramoncjs
# @Time : 2020-11-18 02:35

import requests, re, json, argparse
requests.packages.urllib3.disable_warnings()
result_list = []


def _parse_args():
    parser = argparse.ArgumentParser()
    parser.add_argument(
        "url",
        help="FOFA API's URL.",
    )
    parser.add_argument(
        "-o", '--out',
        help="save Results to file.",
    )
    return parser.parse_args()


def get_data(url):
    print("[+] Running...Please wait.")
    req_url = "{}".format(url).strip()
    try:
        req = requests.get(url=req_url, verify=False)
        result = json.loads(req.text)['results']
        for i in result:
            result_url = re.sub(r"^(?!.*https)", "http://", i[0])
            result_list.append(result_url)
        size = (json.loads(req.text)['size'])
        if size >= 100:
            for i in range(2, (size // 100) + 2):
                print()
                req2 = requests.get(url="{}&page={}".format(req_url, i), verify=False)
                result2 = json.loads(req2.text)['results']
                for i in result2:
                    result_url = re.sub(r"^(?!.*https)", "http://", i[0])
                    result_list.append(result_url)
    except Exception as e:
        print("[-] {}".format(e))
    return result_list


def data_print():
    try:
        chose = input("[+] 共计{}条数据,确定在控制台打印吗？ [Y/N]".format(len(result_list)))
        if (chose.lower() == "y") or (chose == ''):
            for i in result_list:
                print(i)
        else:
            print("[-] Exit.")
    except Exception as e:
        print("[-] {}".format(e))


def data_out(x):
    try:
        with open("{}".format(x), "w+") as f:
            print("[+] Writing...")
            for i in result_list:
                f.write(i + "\n")
            print("[+] Success!")
    except Exception as e:
        print("[-] {}".format(e))


if __name__ == '__main__':
    args = _parse_args()
    print(
        '''
          ____                                   _     
         |  _ \ __ _ _ __ ___   ___  _ __   ___ (_)___ 
         | |_) / _` | '_ ` _ \ / _ \| '_ \ / __|| / __|
         |  _ < (_| | | | | | | (_) | | | | (__ | \__ \\
         |_| \_\__,_|_| |_| |_|\___/|_| |_|\___|/ |___/
                                              |__/     
        '''
    )
    if args.url is None:
        print("[-] Please use -h to get help.")
    elif args.url:
        if args.out:
            get_data(url=args.url)
            data_out(args.out)
        else:
            get_data(url=args.url)
            data_print()
