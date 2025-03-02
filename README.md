# Facebook Scraper

Scrape Facebook public pages without an API key. Inspired by [twitter-scraper](https://github.com/kennethreitz/twitter-scraper).


## Install

```sh
pip install facebook-scraper
```


## Usage

Send the unique **page name, profile name, or ID** as the first parameter and you're good to go:

```python
>>> from facebook_scraper import get_posts

>>> for post in get_posts('nintendo', pages=1):
...     print(post['text'][:50])
...
The final step on the road to the Super Smash Bros
We’re headed to PAX East 3/28-3/31 with new games
```


### Optional parameters

*(For the `get_posts` function)*.

- **group**: group id, to scrape groups instead of pages. Default is `None`.
- **pages**: how many pages of posts to request, the first 2 pages may have no results, so try with a number greater than 2. Default is 10.
- **timeout**: how many seconds to wait before timing out. Default is 5.
- **credentials**: tuple of user and password to login before requesting the posts. Default is `None`.
- **extra_info**: bool, if true the function will try to do an extra request to get the post reactions. Default is False.
- **youtube_dl**: bool, use Youtube-DL for (high-quality) video extraction. You need to have youtube-dl installed on your environment. Default is False.
- **post_urls**: list of strings, URLs to extract posts from. Alternative to fetching based on username.These can either be full URLs or just fragments, either in `{account}/posts/{post_id}` format, or `story.php?story_fbid={post_id}&id={user_id}` format.
- **cookies**: Either the path to a file containing cookies in Netscape format, or a [CookieJar](https://docs.python.org/3.9/library/http.cookiejar.html#http.cookiejar.CookieJar), or a dictionary that can be converted to a CookieJar with [cookiejar_from_dict](https://2.python-requests.org/en/master/api/#requests.cookies.cookiejar_from_dict)
- **options**: Dictionary of options. Set `options={"comments": True}` to extract comments, set `options={"reactors": True}` to extract the people reacting to the post. `reactors` can also be set to a number to set a limit for the amount of reactors to retrieve. Set `options={"allow_extra_requests": False}` to disable making extra requests when extracting post data (required for some things like full text and image links). Set `options={"posts_per_page": 200}` to request 200 posts per page.

## CLI usage

```sh
$ facebook-scraper --filename nintendo_page_posts.csv --pages 10 nintendo
```

Run `facebook-scraper --help` for more details on CLI usage.

**Note:** If you get a `UnicodeEncodeError` try adding `--encoding utf-8`.


## Post example

```python
{'available': True,
 'comments': 459,
 'comments_full': None,
 'factcheck': None,
 'fetched_time': datetime.datetime(2021, 4, 20, 13, 39, 53, 651417),
 'image': 'https://scontent.fhlz2-1.fna.fbcdn.net/v/t1.6435-9/fr/cp0/e15/q65/58745049_2257182057699568_1761478225390731264_n.jpg?_nc_cat=111&ccb=1-3&_nc_sid=8024bb&_nc_ohc=ygH2fPmfQpAAX92ABYY&_nc_ht=scontent.fhlz2-1.fna&tp=14&oh=7a8a7b4904deb55ec696ae255fff97dd&oe=60A36717',
 'images': ['https://scontent.fhlz2-1.fna.fbcdn.net/v/t1.6435-9/fr/cp0/e15/q65/58745049_2257182057699568_1761478225390731264_n.jpg?_nc_cat=111&ccb=1-3&_nc_sid=8024bb&_nc_ohc=ygH2fPmfQpAAX92ABYY&_nc_ht=scontent.fhlz2-1.fna&tp=14&oh=7a8a7b4904deb55ec696ae255fff97dd&oe=60A36717'],
 'is_live': False,
 'likes': 3509,
 'link': 'https://www.nintendo.com/amiibo/line-up/',
 'post_id': '2257188721032235',
 'post_text': 'Don’t let this diminutive version of the Hero of Time fool you, '
              'Young Link is just as heroic as his fully grown version! Young '
              'Link joins the Super Smash Bros. series of amiibo figures!\n'
              '\n'
              'https://www.nintendo.com/amiibo/line-up/',
 'post_url': 'https://facebook.com/story.php?story_fbid=2257188721032235&id=119240841493711',
 'reactions': {'haha': 22, 'like': 2657, 'love': 706, 'sorry': 1, 'wow': 123}, # if `extra_info` was set
 'reactors': None,
 'shared_post_id': None,
 'shared_post_url': None,
 'shared_text': '',
 'shared_time': None,
 'shared_user_id': None,
 'shared_username': None,
 'shares': 441,
 'text': 'Don’t let this diminutive version of the Hero of Time fool you, '
         'Young Link is just as heroic as his fully grown version! Young Link '
         'joins the Super Smash Bros. series of amiibo figures!\n'
         '\n'
         'https://www.nintendo.com/amiibo/line-up/',
 'time': datetime.datetime(2019, 4, 30, 5, 0, 1),
 'user_id': '119240841493711',
 'username': 'Nintendo',
 'video': None,
 'video_id': None,
 'video_thumbnail': None,
 'w3_fb_url': 'https://www.facebook.com/Nintendo/posts/2257188721032235'}
```


### Notes

- There is no guarantee that every field will be extracted (they might be `None`).
- Group posts may be missing some fields like `time` and `post_url`.
- Group scraping may return only one page and not work on private groups.
- If you scrape too much, Facebook might temporarily ban your IP.

## Profiles

The `get_profile` function can extract information from a profile's about section. Pass in the account name or ID as the first parameter.  
Note that Facebook serves different information depending on whether you're logged in (cookies parameter), such as Date of birth and Gender. Usage:

```python
from facebook_scraper import get_profile
get_profile("zuck") # Or get_profile("zuck", cookies="cookies.txt")
```
Outputs:
```python
{'About': "I'm trying to make the world a more open place.",
 'Education': 'Harvard University\n'
              'Computer Science and Psychology\n'
              '30 August 2002 - 30 April 2004\n'
              'Phillips Exeter Academy\n'
              'Classics\n'
              'School year 2002\n'
              'Ardsley High School\n'
              'High School\n'
              'September 1998 - June 2000',
 'Favourite Quotes': '"Fortune favors the bold."\n'
                     '- Virgil, Aeneid X.284\n'
                     '\n'
                     '"All children are artists. The problem is how to remain '
                     'an artist once you grow up."\n'
                     '- Pablo Picasso\n'
                     '\n'
                     '"Make things as simple as possible but no simpler."\n'
                     '- Albert Einstein',
 'Name': 'Mark Zuckerberg',
 'Places lived': [{'link': '/profile.php?id=104022926303756&refid=17',
                   'text': 'Palo Alto, California',
                   'type': 'Current town/city'},
                  {'link': '/profile.php?id=105506396148790&refid=17',
                   'text': 'Dobbs Ferry, New York',
                   'type': 'Home town'}],
 'Work': 'Chan Zuckerberg Initiative\n'
         '1 December 2015 - Present\n'
         'Facebook\n'
         'Founder and CEO\n'
         '4 February 2004 - Present\n'
         'Palo Alto, California\n'
         'Bringing the world closer together.'}
```

To extract friends, pass the argument `friends=True`, or to limit the amount of friends retrieved, set `friends` to the desired number.

## To-Do

- Async support
- ~~Image galleries~~ (`images` entry)
- ~~Profiles or post authors~~ (`get_profile()`)
- ~~Comments~~ (with `options={'comments': True}`)


## Alternatives and related projects

- [facebook-post-scraper](https://github.com/brutalsavage/facebook-post-scraper). Has comments. Uses Selenium.
- [facebook-scraper-selenium](https://github.com/apurvmishra99/facebook-scraper-selenium). "Scrape posts from any group or user into a .csv file without needing to register for any API access".
- [Ultimate Facebook Scraper](https://github.com/harismuneer/Ultimate-Facebook-Scraper).  "Scrapes almost everything about a Facebook user's profile". Uses Selenium.
- [Unofficial APIs](https://github.com/Rolstenhouse/unofficial-apis). List of unofficial APIs for various services, none for Facebook for now, but might be worth to check in the future.
- [major-scrapy-spiders](https://github.com/talhashraf/major-scrapy-spiders). Has a profile spider for Scrapy.
- [facebook-page-post-scraper](https://github.com/minimaxir/facebook-page-post-scraper). Seems abandoned.
    - [FBLYZE](https://github.com/isaacmg/fb_scraper). Fork (?).
- [RSSHub](https://github.com/DIYgod/RSSHub/blob/master/lib/routes/facebook/page.js). Generates an RSS feed from Facebook pages.
- [RSS-Bridge](https://github.com/RSS-Bridge/rss-bridge/blob/master/bridges/FacebookBridge.php). Also generates RSS feeds from Facebook pages.
