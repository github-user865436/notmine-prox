import sys

# Function return
def clist(str, gvars, returning, func):
    variables = gvars
    for cindex in range(1, len(str) + 1):
        func(cindex, variables, lambda changes: {variables.update(changes)})
    return variables[returning]


# Encode text
def converturl(url):
    return clist(
        url,
        {
            "editurl": "",
            "reserve": -1,
        },
        "editurl",
        lambda cindex, variables, change: (
            change(
                {"reserve": abs(variables["reserve"] + 0.5) - 1.5}
            ),  # had to: (+, then -) 1
            (
                (
                    lambda cbyte: (
                        change({"reserve": variables["reserve"] + 2}),
                        change(
                            {
                                "editurl": variables["editurl"]
                                + chr(
                                    cbyte + (2 * (-1 if (cindex / 2) % 2 == 0 else 1))
                                )
                            }
                        ),
                    )
                    if cbyte == 37
                    else change({"editurl": variables["editurl"] + chr(cbyte)})
                )(ord(url[cindex - 1]))
            )
            if variables["reserve"] < 0
            else None,
        ),
    )


# Recommended input method
use = 1
links = [
    "https://interstellar-five-red.vercel.app/a/",
    "https://use.moldeo.org/@/space/",
]
links.insert(0, "")

url = input()
if url == "":
    # Split string function
    dfl = {"opc": None, "str": None}

    def split(sct, opc, str):
        nonlocal dfl
        if sct != 0:
            u = {"opc": None, "str": None}
            if opc is None or str is None:
                u = dfl
            else:
                u = {"opc": opc, "str": str}

            return clist(
                u["str"],
                {
                    "u": u,
                    "scts": [],
                },
                "scts",
                lambda cindex, variables, change: (
                    (variables["scts"].append(""), None)[0]
                    if u["str"][cindex - 1] == u["opc"]
                    else variables["scts"].__setitem__(
                        -1, variables["scts"][-1] + u["str"][cindex - 1]
                    )
                ),
            )[sct]
        else:
            dfl = {"opc": opc, "str": str}

    # URL sections
    spl = "s"
    opcs = f"{spl}://{spl}.{spl}.{spl}:{spl}/{spl}?{spl}#{spl}"

    split(0, spl, opcs)
    # you can use this if you don't want to copy and paste url
    dfurl = {
        "Scheme": ["https", "", 0],
        "Sub-Domain": ["www", split(1), 1],
        "Domain": ["roblox", split(2), 2],
        "TLD": ["com", split(3), 3],
        "Port": ["", split(4), 4],
        "Path": [[], split(5), 5],
        "Query": [[], split(6), 6],
        "Fragment": ["", split(7), 7],
    }
    split(0)

    Path, Query = "", ""
    for SubDirectory in dfurl["Path"][1]:
        Path += SubDirectory + "/"

    for Key, Value in dfurl["Query"][1].items():
        Query += f"{Key}={Value}&"

    dfurl["Path"][1], dfurl["Query"][1] = Path, Query[:-1]

    # Combine sections
    sortedparts = []
    for Name, Value in dfurl.items():
        if Value[1] != "":
            sortedparts.insert(Value[3] + 1, Value[2] + Value[1])

    for part in sortedparts:
        url += part

    # Return encoding
    print(links[use] + converturl(url))
