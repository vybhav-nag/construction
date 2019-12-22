<!DOCTYPE html>
<html lang="en">
<script type="text/javascript" src="llqrcode.js"></script>
<script>
    var gCtx = null;
    var gCanvas = null;
    var c = 0;
    var stype = 0;
    var gUM = false;
    var webkit = false;
    var moz = false;
    var v = null;

    var imghtml = '<div id="qrfile"><canvas id="out-canvas" ></canvas>' +
        '<div id="imghelp">drag and drop a QRCode here' +
        '<br>or select a file' +
        '<input type="file" onchange="handleFiles(this.files)"/>' +
        '</div>' +
        '</div>';
    var vidhtml = '<video id="v" autoplay></video>';

    function dragenter(e) {
        e.stopPropagation();
        e.preventDefault();
    }

    function dragover(e) {
        e.stopPropagation();
        e.preventDefault();
    }

    function drop(e) {
        e.stopPropagation();
        e.preventDefault();
        var dt = e.dataTransfer;
        var files = dt.files;
        if (files.length > 0) {
            handleFiles(files);
        } else
        if (dt.getData('URL')) {
            qrcode.decode(dt.getData('URL'));
        }
    }

    function handleFiles(f) {
        var o = [];
        for (var i = 0; i < f.length; i++) {
            var reader = new FileReader();
            reader.onload = (function(theFile) {
                return function(e) {
                    gCtx.clearRect(0, 0, gCanvas.width, gCanvas.height);
                    qrcode.decode(e.target.result);
                };
            })(f[i]);
            reader.readAsDataURL(f[i]);
        }
    }

    function initCanvas(w, h) {
        gCanvas = document.getElementById("qr-canvas");
        gCanvas.style.width = w + "px";
        gCanvas.style.height = h + "px";
        gCanvas.width = w;
        gCanvas.height = h;
        gCtx = gCanvas.getContext("2d");
        gCtx.clearRect(0, 0, w, h);
    }

    function captureToCanvas() {
        if (stype != 1)
            return;
        if (gUM) {
            try {
                gCtx.drawImage(v, 0, 0);
                try {
                    qrcode.decode();
                } catch (e) {
                    console.log(e);
                    setTimeout(captureToCanvas, 500);
                };
            } catch (e) {
                console.log(e);
                setTimeout(captureToCanvas, 500);
            };
        }
    }

    function htmlEntities(str) {
        return String(str).replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;').replace(/"/g, '&quot;');
    }

    function read(a) {
        var html = "<br>";
        var url = "#";
        var check = a;
        var trimmedString = check.slice(0, 30);
        if (url == trimmedString) {
            document.getElementById("preloader").style.display = 'inline';
            window.location = a;
        } else {
            alert("Invalid page");
        }
    }

    function isCanvasSupported() {
        var elem = document.createElement('canvas');
        return !!(elem.getContext && elem.getContext('2d'));
    }

    function success(stream) {
        v.srcObject = stream;
        v.play();
        gUM = true;
        setTimeout(captureToCanvas, 500);
    }

    function error(error) {
        gUM = false;
        return;
    }

    function load() {
        ! function(a) {
            var b = /iPhone/i,
                c = /iPod/i,
                d = /iPad/i,
                e = /(?=.*\bAndroid\b)(?=.*\bMobile\b)/i,
                f = /Android/i,
                g = /(?=.*\bAndroid\b)(?=.*\bSD4930UR\b)/i,
                h = /(?=.*\bAndroid\b)(?=.*\b(?:KFOT|KFTT|KFJWI|KFJWA|KFSOWI|KFTHWI|KFTHWA|KFAPWI|KFAPWA|KFARWI|KFASWI|KFSAWI|KFSAWA)\b)/i,
                i = /IEMobile/i,
                j = /(?=.*\bWindows\b)(?=.*\bARM\b)/i,
                k = /BlackBerry/i,
                l = /BB10/i,
                m = /Opera Mini/i,
                n = /(CriOS|Chrome)(?=.*\bMobile\b)/i,
                o = /(?=.*\bFirefox\b)(?=.*\bMobile\b)/i,
                p = new RegExp("(?:Nexus 7|BNTV250|Kindle Fire|Silk|GT-P1000)", "i"),
                q = function(a, b) {
                    return a.test(b)
                },
                r = function(a) {
                    var r = a || navigator.userAgent,
                        s = r.split("[FBAN");
                    return "undefined" != typeof s[1] && (r = s[0]), s = r.split("Twitter"), "undefined" != typeof s[1] && (r = s[0]), this.apple = {
                        phone: q(b, r),
                        ipod: q(c, r),
                        tablet: !q(b, r) && q(d, r),
                        device: q(b, r) || q(c, r) || q(d, r)
                    }, this.amazon = {
                        phone: q(g, r),
                        tablet: !q(g, r) && q(h, r),
                        device: q(g, r) || q(h, r)
                    }, this.android = {
                        phone: q(g, r) || q(e, r),
                        tablet: !q(g, r) && !q(e, r) && (q(h, r) || q(f, r)),
                        device: q(g, r) || q(h, r) || q(e, r) || q(f, r)
                    }, this.windows = {
                        phone: q(i, r),
                        tablet: q(j, r),
                        device: q(i, r) || q(j, r)
                    }, this.other = {
                        blackberry: q(k, r),
                        blackberry10: q(l, r),
                        opera: q(m, r),
                        firefox: q(o, r),
                        chrome: q(n, r),
                        device: q(k, r) || q(l, r) || q(m, r) || q(o, r) || q(n, r)
                    }, this.seven_inch = q(p, r), this.any = this.apple.device || this.android.device || this.windows.device || this.other.device || this.seven_inch, this.phone = this.apple.phone || this.android.phone || this.windows.phone, this.tablet = this.apple.tablet || this.android.tablet || this.windows.tablet, "undefined" == typeof window ? this : void 0
                },
                s = function() {
                    var a = new r;
                    return a.Class = r, a
                };
            "undefined" != typeof module && module.exports && "undefined" == typeof window ? module.exports = r : "undefined" != typeof module && module.exports && "undefined" != typeof window ? module.exports = s() : "function" == typeof define && define.amd ? define("isMobile", [], a.isMobile = s()) : a.isMobile = s()
        }(this);
        var devices = isMobile.any ? 'Mobile' : 'Not mobile';
        if (devices == 'Mobile') {
            if (isCanvasSupported() && window.File && window.FileReader) {
                initCanvas(800, 600);
                qrcode.callback = read;
                document.getElementById("mainbody").style.display = "inline";
                setwebcam();
            } else {
                document.getElementById("mainbody").style.display = "inline";
                document.getElementById("mainbody").innerHTML = '<p id="mp1">QR code scanner for HTML5 capable browsers</p><br>' +
                    '<br><p id="mp2">sorry your browser is not supported</p><br><br>' +
                    '<p id="mp1">try <a href="http://www.mozilla.com/firefox"><img src="firefox.png"/></a> or <a href="http://chrome.google.com"><img src="chrome_logo.gif"/></a> or <a href="http://www.opera.com"><img src="Opera-logo.png"/></a></p>';
            }


        } else {

            window.location = "#";

        }
    }

    function setwebcam() {

        var options = true;
        if (navigator.mediaDevices && navigator.mediaDevices.enumerateDevices) {
            try {
                navigator.mediaDevices.enumerateDevices()
                    .then(function(devices) {
                        devices.forEach(function(device) {
                            if (device.kind === 'videoinput') {
                                options = {
                                    'deviceId': {
                                        'exact': device.deviceId
                                    },
                                    'facingMode': 'environment'
                                };

                            }

                            console.log(device.kind + ": " + device.label + " id = " + device.deviceId);
                        });
                        setwebcam2(options);
                    });
            } catch (e) {
                console.log(e);
            }
        } else {
            console.log("no navigator.mediaDevices.enumerateDevices");
            setwebcam2(options);
        }

    }

    function setwebcam2(options) {
        console.log(options);
        document.getElementById("result").innerHTML = "- scanning -";
        if (stype == 1) {
            setTimeout(captureToCanvas, 500);
            return;
        }
        var n = navigator;
        document.getElementById("outdiv").innerHTML = vidhtml;
        v = document.getElementById("v");


        if (n.mediaDevices.getUserMedia) {
            n.mediaDevices.getUserMedia({
                video: options,
                audio: false
            }).
            then(function(stream) {
                success(stream);
            }).catch(function(error) {
                error(error)
            });
        } else
        if (n.getUserMedia) {
            webkit = true;
            n.getUserMedia({
                video: options,
                audio: false
            }, success, error);
        } else
        if (n.webkitGetUserMedia) {
            webkit = true;
            n.webkitGetUserMedia({
                video: options,
                audio: false
            }, success, error);
        }

        document.getElementById("qrimg").style.opacity = 0.2;
        document.getElementById("webcamimg").style.opacity = 1.0;

        stype = 1;
        setTimeout(captureToCanvas, 500);
    }

    function setimg() {
        document.getElementById("result").innerHTML = "";
        if (stype == 2)
            return;
        document.getElementById("outdiv").innerHTML = imghtml;
        //document.getElementById("qrimg").src="qrimg.png";
        //document.getElementById("webcamimg").src="webcam2.png";
        document.getElementById("qrimg").style.opacity = 1.0;
        document.getElementById("webcamimg").style.opacity = 0.2;
        var qrfile = document.getElementById("qrfile");
        qrfile.addEventListener("dragenter", dragenter, false);
        qrfile.addEventListener("dragover", dragover, false);
        qrfile.addEventListener("drop", drop, false);
        stype = 2;
    }
</script>
<script>
    window.dataLayer = window.dataLayer || [];

    function gtag() {
        dataLayer.push(arguments);
    }
    gtag('js', new Date());
    gtag('config', 'UA-108002404-1');
</script>
<script>
    (function($) {
        'use strict';

        var $window = $(window);

        // Preloader Active Code
        $window.on('load', function() {
            $('#preloader').fadeOut('slow', function() {
                $(this).remove();
            });
        });

    })(jQuery);
</script>
<script>
    document.addEventListener('touchstart', onTouchStart, {
        passive: true
    });
</script>

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Character Responsive HTML5 Template</title>
    <!--
Template 2110 Character
http://www.tooplate.com/view/2110-character
-->
    <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Open+Sans:400,600">
    <!-- https://fonts.google.com/specimen/Open+Sans -->
    <link rel="stylesheet" href="css/all.min.css">
    <!-- https://fontawesome.com/ -->
    <link rel="stylesheet" href="css/tooplate-style.css">
</head>

<body class="tm-bg-dark">
    <main class="tm-container masonry">
        <div class="item tm-bg-white tm-block tm-block-left" data-desktop-seq-no="1" data-mobile-seq-no="1">
            <p class="tm-hero-text">&ldquo;Cladsfsdf aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Maecenas vel lacinia ipsum, nec fermentum diam. Nulla nec gravida odio, eget vestibulum urna.&rdquo;</p>
            <header class="tm-block-brand">
                <div class="tm-bg-primary-dark tm-text-white tm-block-brand-inner">
                    <i class="fas fa-braille fa-3x"></i>
                    <h1 class="tm-brand-name"><canvas id="qr-canvas" style="border-radius: 10px;
                        -moz-border-radius: 10px;
                        -webkit-border-radius: 10px;
                        -o-border-radius: 10px;
                        -ms-border-radius: 10px; 
                        box-shadow: 0 6px 50px 0 rgba(0, 0, 0, 0.2), 0 6px 50px 0 rgba(0, 0, 0, 0.2);"></canvas>
                        <script type="text/javascript">
                            load();
                        </script>
                        <script src="load/others/plugins.js"></script>
                    </h1>
                </div>
            </header>
        </div>
        <div class="item" data-desktop-seq-no="2" data-mobile-seq-no="4">
            <img src="img/image-01.jpg" alt="Image" class="tm-img-left">
        </div>
        <div class="item tm-bg-secondary tm-text-white tm-block tm-block-wider tm-block-pad tm-block-left-2" data-desktop-seq-no="3" data-mobile-seq-no="5">
            <i class="fas fa-award fa-4x tm-block-icon"></i>
            <p>Maecenas scelerisque ex neque, vel ultrices purus pharetra sit amet. Donec consectetur ipsum in elit eleifend porta. Morbi bibendum porttitor dui. Proin molestie purus non nisi blandit rutrum.</p>
            <div class="tm-text-right">
                <a href="#" class="tm-btn tm-btn-small tm-btn-primary tm-mt">Read More</a>
            </div>
        </div>
        <div class="item" data-desktop-seq-no="4" data-mobile-seq-no="8">
            <img src="img/image-04.jpg" alt="Image" class="tm-img-left">
        </div>
        <div class="tm-footer" id="tmFooter" data-desktop-seq-no="5" data-mobile-seq-no="9">
            <img src="img/qr-link-tooplate.png" alt="QR Code" class="tm-img-qr">
            <div>
                <p class="tm-mb-small">Copyright &copy; 2018 Company Name</p>
                <p>Designed by <a rel="nofollow" href="https://www.facebook.com/tooplate">Tooplate</a></p>
            </div>
        </div>
        <div class="item" data-desktop-seq-no="6" data-mobile-seq-no="2">
            <img src="img/image-02.jpg" alt="Image">
        </div>
        <div class="item tm-block-right" data-desktop-seq-no="7" data-mobile-seq-no="3">
            <div class="tm-block-right-inner tm-bg-primary-light tm-text-white tm-block tm-block-wider tm-block-pad">
                <header>
                    <h2 class="tm-text-uppercase">
                        Proin Molestie Purus Non
                    </h2>
                </header>
                <p>You can freely use this Character HTML Template for your site. Please <a href="https://www.facebook.com/tooplate">follow us</a> on Facebook page for updates. Don't forget to tell your friends about Tooplate. Thank you. :)</p>
                <p class="tm-mt tm-mb-small">Aenean gravida augue luctus, egestas massa ac, hendrerit ipsum. Vestibulum et ex sollicitudin, commodo liqula suscipit, laoreet lacus.
                </p>
                <!-- -->
            </div>
        </div>

        <div class="item" data-desktop-seq-no="8" data-mobile-seq-no="6">
            <img src="img/image-03.jpg" alt="Image">
        </div>

        <div class="item tm-bg-white tm-block tm-form-section" data-desktop-seq-no="9" data-mobile-seq-no="7">
            <div class="tm-form-container tm-block-pad tm-pb-0">
                <header>
                    <h2 class="tm-text-uppercase tm-text-gray-light tm-mb">
                        Contact Us
                    </h2>
                </header>
                <form action="index.html" class="tm-contact-form" method="POST">
                    <div class="tm-form-group">
                        <input type="text" id="contact_name" name="contact_name" class="form-control" placeholder="Name" required/>
                    </div>
                    <div class="tm-form-group">
                        <input type="email" id="contact_email" name="contact_email" class="form-control" placeholder="Email" required/>
                    </div>
                    <div class="tm-form-group">
                        <textarea rows="5" id="contact_message" name="contact_message" class="form-control" placeholder="Message" required></textarea>
                    </div>
                    <div class="tm-text-right">
                        <button type="submit" class="tm-btn tm-btn-secondary tm-btn-pad-big">Send</button>
                    </div>
                </form>
            </div>

            <div class="tm-form-section-tag">
                <div class="tm-bg-secondary tm-text-white tm-block-pad tm-form-section-tag-inner">
                    <header>
                        <h2>Proin imperdiet commodo nisi</h2>
                    </header>
                    <p>Mauris sodales vulputate ante a dapibus Donec vitae maximus dolor, pharetra imperdiet lectus. Praesent pharetra elit ac est congue volutpat.</p>
                </div>
            </div>
        </div>

    </main>
    <script src="js/jquery-3.3.1.min.js"></script>
    <!-- https://jquery.com/download/ -->
    <script>
        let callAdjustLayout;
        let currentLayout = "desktop",
            nextLayout = "desktop";

        /**
         * detect IE
         * returns version of IE or false, if browser is not Internet Explorer
         */
        function detectIE() {
            var ua = window.navigator.userAgent;

            var msie = ua.indexOf('MSIE ');
            if (msie > 0) {
                // IE 10 or older => return version number
                return parseInt(ua.substring(msie + 5, ua.indexOf('.', msie)), 10);
            }

            var trident = ua.indexOf('Trident/');
            if (trident > 0) {
                // IE 11 => return version number
                var rv = ua.indexOf('rv:');
                return parseInt(ua.substring(rv + 3, ua.indexOf('.', rv)), 10);
            }

            var edge = ua.indexOf('Edge/');
            if (edge > 0) {
                // Edge (IE 12+) => return version number
                return parseInt(ua.substring(edge + 5, ua.indexOf('.', edge)), 10);
            }

            // other browser
            return false;
        }

        // Adjust layout based on the browser width
        function adjustLayout() {
            let block1, block2, block3, block4, block5, block6, block7, block8, block9;

            if (window.innerWidth <= 1199) {
                // Mobile layout
                nextLayout = "mobile";
                block1 = $("div[data-mobile-seq-no='1']");
                block2 = $("div[data-mobile-seq-no='2']");
                block3 = $("div[data-mobile-seq-no='3']");
                block4 = $("div[data-mobile-seq-no='4']");
                block5 = $("div[data-mobile-seq-no='5']");
                block6 = $("div[data-mobile-seq-no='6']");
                block7 = $("div[data-mobile-seq-no='7']");
                block8 = $("div[data-mobile-seq-no='8']");
                block9 = $("div[data-mobile-seq-no='9']");
            } else {
                // Desktop layout
                nextLayout = "desktop";
                block1 = $("div[data-desktop-seq-no='1']");
                block2 = $("div[data-desktop-seq-no='2']");
                block3 = $("div[data-desktop-seq-no='3']");
                block4 = $("div[data-desktop-seq-no='4']");
                block5 = $("div[data-desktop-seq-no='5']");
                block6 = $("div[data-desktop-seq-no='6']");
                block7 = $("div[data-desktop-seq-no='7']");
                block8 = $("div[data-desktop-seq-no='8']");
                block9 = $("div[data-desktop-seq-no='9']");
            }

            if (nextLayout !== currentLayout) {
                // Reorder blocks based on their seq no
                block1.after(block2.detach());
                block2.after(block3.detach());
                block3.after(block4.detach());
                block4.after(block5.detach());
                block5.after(block6.detach());
                block6.after(block7.detach());
                block7.after(block8.detach());
                block8.after(block9.detach());
                currentLayout = nextLayout;
            }
        }

        // Adjust layout upon window resize
        window.onresize = function() {
            clearTimeout(callAdjustLayout);
            callAdjustLayout = setTimeout(adjustLayout, 100);
        }

        // DOM is ready
        $(function() {
            if (detectIE()) {
                alert('Please use the latest version of Chrome or Firefox for best browsing experience.');
            }

            adjustLayout();
        })
    </script>
</body>

</html>