follow-discussion-hn
====================

This is a pretty simple JS plugin to retrieve the HN comment thread for a given URL. The motivation behind the plugin was that I've been reading a lot of posts recently that have a "Follow this discussion on Hacker News" link at the bottom but until you submit the post you can't really add that link. The plugin tries to solve that.

This is the approach:
1. Use Firebase to store the links. Since it can be used via client side JS it doesn't require running a backend server.
2. Use the http://hndroidapi.appspot.com/ API to retrieve a user's recent submissions and then find the match.

In hindsight, it's not really necessary to have Firebase here at all except for efficiency's sake and to get to play around with it.

Plugin Limitations:
1. It will only look at a user's most recent submissions but after the first retrieval it should be stored in Firebase.
2. It depends on both Firebase and an unofficial HN API so either of those can break at any moment.
3. Since the Firebase location is public it's pretty easy for someone to overwrite the data. Authentication, which Firebase seems to support, would fix this.
4. This is my first "real" Javascript plugin so use at your own risk.

Sample page:

    <html>
    <head>
      <title>Follow Discussion on HN Plugin</title>
      <script src='https://cdn.firebase.com/v0/firebase.js'></script>
    </head>
    <body>
      <div class="result"></div>

      <script src="//ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>
      <script src="js/hn.follow.js"></script>
      <script type="text/javascript">
        $(document).ready(function(){
          var loc = window.location;
          var curr_url = loc.protocol + '//' + loc.host + loc.pathname;

          var hn = new $.followHN({
            username: 'dangoldin',
            firebase_url: 'https://follow-on-hn.firebaseio.com/urls',
            callback: function(thread_url) {
              $('.result').html('Follow discussion on <a href="' + thread_url + '">Hacker News</a>');
            }
          });
          // hn.get_hn_url('http://dangoldin.com/2013/04/12/why-dont-cellphones-have-a-dialtone/');
          // hn.get_hn_url('http://blogs.hbr.org/hbr/hbreditors/2013/03/power_of_visualizations_aha_moment.html');
          hn.get_hn_url(curr_url);
        });
      </script>
    </body>
    </html>