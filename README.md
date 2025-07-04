[![MethodsHub Guidelines](https://img.shields.io/badge/MethodsHub_Methods-GuidelinesV3-purple)](https://github.com/GESIS-Methods-Hub/guidelines-for-methods/)
![](https://img.shields.io/badge/License-MIT-blue)
 
# 4TCT: A 4chan Text Collection Tool

## Description
4TCT is a specialized tool designed for the efficient collection of textual data from the [4chan](https://www.4chan.org/) platform. It automates the process of gathering posts from various boards, aiming to facilitate research and analysis in social science and computational linguistics.

This tool is particularly useful for analyzing online discourse, community dynamics, and trends within the 4chan ecosystem. It can support research on topics such as *hate speech*, *conspiracy theories*, *online extremism*, *meme culture*, *information dissemination*, and the impact of anonymous social media on public opinion. The research paper [User unknown: 4chan, anonymity and contingency](https://firstmonday.org/ojs/index.php/fm/article/view/3665/8696) investigates anonymity and contingency aspects of 4chan in keeping its users unknown.

## Use Case(s)
- A **social scientist** analyzes the prevalence and evolution of hate speech and extremist narratives in online communities. They use 4TCT to collect posts from various 4chan boards to study patterns and triggers for such discourse.
- A **research team** investigates how conspiracy theories emerge and spread during political events. Using 4TCT, they gather data to identify key narratives and influential threads on 4chan boards.
- A **computational linguist** leverages 4TCT to build a corpus for training models on internet slang, meme-based text, and the language of conspiracy theories.

## Input Data
Not applicable as the tool dynamically gathers live data directly from 4chan boards based on user-defined parameters.

## Output 
Outputs include `.json` files containing collected posts, structured according to 4chan's API documentation, with directories organized by date and board.

```json
{
  "posts": [
    {
      "no": 1990691,
      "sticky": 1,
      "closed": 1,
      "now": "02/23/13(Sat)22:43",
      "name": "Anonymous",
      "sub": "/c/ board rules and guidelines",
      "com": "Greetings, /c/itizens!<br><br>Just wanted to go over a couple of guidelines for posting on /c/ if you are new here and a friendly reminder for those who aren&#039;t. <br><br>Try to avoid making single-image requests. Making a single image thread deletes a previous thread from the last page. Please include 4 to 5 similar images in your thread to get it going. Use resources like danbooru, gelbooru or even Google Image Search. <br><br>Check the catalog to avoid making a duplicate thread. This way, we can share and contribute more images more effectively and efficiently. <br><br>If bumping a thread, please include a picture instead of just writing &#039;bump&#039; and please do not necrobump threads that have reached their image limit. This restricts the diversity and natural flow of the board. Threads are meant to come and go and sometimes they are even better the next time around.<br><br>Finally, as much as /a/ is a discussion board, /c/ is a board for sharing images. Please respect the threads of other users and they will do the same to yours as well!",
      "filename": "1327087650882",
      "ext": ".jpg",
      "w": 662,
      "h": 1000,
      "tn_w": 165,
      "tn_h": 250,
      "tim": 1361677439762,
      "time": 1361677439,
      "md5": "gP8R0+qEv/MBLCVaFcKY9Q==",
      "fsize": 325353,
      "resto": 0,
      "semantic_url": "c-board-rules-and-guidelines",
      "replies": 0,
      "images": 0,
      "unique_ips": 1
    }
  ],
  "last_modified": 1405561559,
  "archived": false,
  "post_time_UTC": "13_02_24_03_43_00",
  "scraped_time_UTC": "23_11_24_13_03_09",
  "board_code": "c"
}
```

For explaination of the fields in the downloaded `.json` file, refer to [4chan API page](https://github.com/4chan/4chan-API/blob/master/pages/Threads.md)

## Hardware Requirements
The method require dedicated server(s) with enough capacity (depending on parameters settings) to store the data.

## Environment Setup
It Requires Python>3.10.2 Suitable for environments focused on data collection and analysis.

Dependencies are listed in [requirements.txt](https://github.com/BDA-KTS/4CTC/blob/main/requirements.txt) and can be installed via `pip install -r requirements.txt` to ensure the tool functions correctly.

## How to Use
You can run 4TCT from the command line to start collecting threads from specific 4chan boards.
- Run `python src/requester.py` to start data collection, with options `-b` for board selection and `-e` for board exclusion. Advanced usage includes adjusting request intervals and logging levels for detailed monitoring.

Use the `-b` option to specify which boards to scrape:

```bash
python src/requester.py -b a g sci
```

This collects threads from the `/a/` (Anime), `/g/` (Technology), and `/sci/` (Science & Math) boards.

To **exclude** certain boards instead of including them, add the `-e` flag:

```bash
python src/requester.py -b pol -e
```

This collects threads from all boards **except** `/pol/`.

To view all available options, run:

```bash
python src/requester.py -h
```
- To use a configuration file instead of command-line arguments, add the `-c` flag without any other arguments. (i.e. `python src/requester.py -c`) This will read settings from a [config.json](https://github.com/BDA-KTS/4CTC/blob/main/config.json) file located the root folder. 
  The configuration file should be structured as follows:
    ```json
    {
        "boards": [], 
        "exclude_boards": false,
        "request_time_limit": 1,
        "output_path": "",
        "save_log": false,
        "clean_log": false
    }
    ```
    - **`boards`**: A list of board short codes to monitor. If left as an empty list (`[]`), all boards will be monitored.
    - **`exclude_boards`**: If `true`, the boards listed in `boards` will be excluded, and all others will be monitored.
    - **`request_time_limit`**: The minimum time (in seconds) between requests to avoid overloading the server. Must be 1 or greater.
    - **`output_path`**: Path to the directory where scraped threads and logs will be saved. A `data` folder will be created inside this path for storing results. If set to `""`, this will save the output `data` folder in the root folder of the repository.
    - **`save_log`**: If `true`, logs will be saved in a `log` folder under the specified `output_path`.
    - **`clean_log`**: If `true`, logs older than three days will be automatically cleaned up.
      
**Where to Find Board Codes**:
  The short codes for 4chan boards can be found on the url of each [4chan boards page](https://boards.4chan.org). For example:
    - `/a/` for Anime & Manga
    - `/g/` for Technology
    - `/sci/` for Science & Math
    
   Simply use the code without the slashes in the `boards` field or with the `-b` option. For instance:
    ```bash
    python src/requester.py -b a g sci
    ```
    or in the configuration file:
    ```json
    {
        "boards": ["a", "g", "sci"],
    }
    ```

  For more information, run:
    ```bash
    python src/requester.py -h
    ```
    
To initialize:
  - Two directories are created for logs, and the data (saves/"the current date")
  - The requester will first query the 4chan API to find the current list of boards, if present the include or exclude boards are selected or removed from the list. For every board resulting from this process, two subdirectories folder will be created in the data folder, one for storing the threads and one for the thread on each board.
  - The requester then goes through each board to find a list of threads on each board. These are saved to the threads_on_boards folder
  - The requester then requests the posts on each board. The data is saved to a subfolder of threads, with a name consisting of the thread id and the time of first observance.
  - The loop repeats by checking each board for new and dead threads, then querying the new and live threads.   
  - **Rerun:** The requester attempts to pick up from previous runs by observing the state of the saves directory. If this is deleted it will act as from fresh.
  - **Logs:** Debug logs are set to capture each API call and are as such, very detailed (approx 80 times as large as info). By default the info log is output to terminal.

## References
Thank you very much to the team behind the [4chan API](https://github.com/4chan/4chan-API)!

The associated technical report is available at:
 Culbert, J. H. (2023). 4TCT, A 4chan Text Collection Tool. arXiv preprint arXiv:2307.03556. [arXiv:2307.03556](https://arxiv.org/abs/2307.03556).
*Users are encouraged to cite this paper when using the tool in research.*

## Acknowledgements
Special thanks to **Jack Culbert**, the original creator of this repository, for laying the foundation of this project.  
Deep appreciation to **Po-Chun Chang**, who, through iterative improvements, expanded the utility and structure of the repository, making it more robust and publishable.  
Gratitude is also extended to the **[4chan API team](https://github.com/4chan)** for providing the foundational resources that enable this tool's functionality.  

## Disclaimer
The creators of 4TCT and GESIS are not affiliated with 4chan. The tool is intended for academic research, and users are responsible for ensuring the legality and ethicality of their data use.

API Rules: Below official API rules have been made as default setting for this repo. They are listed here for those who are interested in modifying the repo.
1. Do not make more than one request per second. To change the waiting time, use `--request-time-limit {your_ideal_value}` flag to set your ideal waiting time (only value above 1 will be accepted).
2. Thread updating should be set to a minimum of 10 seconds, preferably higher.
3. Use [If-Modified-Since](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Modified-Since) when doing your requests.
4. Make API requests using the same protocol as the app. Only use SSL when a user is accessing your app over HTTPS.

Please ensure you follow the 4chan API Rules and Terms of Service found [here](https://github.com/4chan/4chan-API/blob/master/README.md).

## Contact Details
For questions or contributions, contact Jack H. Culbert at [jack.culbert@gesis.org](mailto:jack.culbert@gesis.org) and Po-Chun Chang for maintenance issues at [po-chun.chang@gesis.org](mailto:po-chun.chang@gesis.org).

