/*
    Tabs JavaScript from Dynamic Web Coding at dyn-web.com
    Copyright 2001-2013 by Sharon Paine
    For demos, documentation and updates, visit http://www.dyn-web.com/code/tabs/

    Released under the MIT license
    http://www.dyn-web.com/business/license.txt
*/

// DYN_WEB is namespace used for code from dyn-web.com
// replacing previous use of dw_ prefix for object names
var DYN_WEB = DYN_WEB || {};

/*
    dw_event.js - basic event handling file from dyn-web.com
    version date May 2013 (added .domReady)
    .domReady uses whenReady fn from JavaScript the Definitive Guide
    6th edition by David Flanagan, example 17.01
*/

DYN_WEB.Event=(function(Ev){Ev.add=document.addEventListener?function(obj,etype,fp,cap){cap=cap||false;obj.addEventListener(etype,fp,cap);}:function(obj,etype,fp){obj.attachEvent('on'+etype,fp);};Ev.remove=document.removeEventListener?function(obj,etype,fp,cap){cap=cap||false;obj.removeEventListener(etype,fp,cap);}:function(obj,etype,fp){obj.detachEvent('on'+etype,fp);};Ev.DOMit=function(e){e=e?e:window.event;if(!e.target){e.target=e.srcElement;}if(!e.preventDefault){e.preventDefault=function(){e.returnValue=false;return false;};}if(!e.stopPropagation){e.stopPropagation=function(){e.cancelBubble=true;};}return e;};Ev.getTarget=function(e){e=Ev.DOMit(e);var tgt=e.target;if(tgt.nodeType!==1){tgt=tgt.parentNode;}return tgt;};Ev.domReady=(function(){var funcs=[];var ready=false;function handler(e){if(ready){return;}if(e.type==="readystatechange"&&document.readyState!=="complete"){return;}for(var i=0,len=funcs.length;i<len;i++){funcs[i].call(document);}ready=true;funcs=[];}if(document.addEventListener){document.addEventListener("DOMContentLoaded",handler,false);document.addEventListener("readystatechange",handler,false);window.addEventListener("load",handler,false);}else if(document.attachEvent){document.attachEvent("onreadystatechange",handler);window.attachEvent("onload",handler);}return function whenReady(f){if(ready){f.call(document);}else{funcs.push(f);}};})();return Ev;})(DYN_WEB.Event||{});

DYN_WEB.Cookie={set:function(name,value,expiry,path,domain,secure){var date,expires='';if(typeof expiry==="number"){date=new Date();date.setTime(date.getTime()+(expiry*24*60*60*1000));expires=date.toUTCString();}else if(expiry instanceof Date){expires=expiry.toUTCString();}document.cookie=encodeURIComponent(name)+'='+encodeURIComponent(value)+(expires?'; expires='+expires:'')+(path?'; path='+path:'')+(domain?'; domain='+domain:'')+(secure?'; secure':'');},get:function(name){var c,cookies=document.cookie.split(/;\s/g);for(var i=0,len=cookies.length;i<len;i++){c=cookies[i];if(c.indexOf(name+'=')===0){return decodeURIComponent(c.slice(name.length+1));}}return '';},del:function(name,path,domain,secure){this.set(name,'',new Date(0),path,domain,secure);}};

DYN_WEB.Util = (function( Ut ) {
    
    Ut.$ = function( id ) {
        return document.getElementById( id );
    };
    
    Ut.trim = function( str ) {
        if ( String.prototype.trim ) {
            return str.trim(); // ECMAScript 5
        } else {
            var re = /^\s+|\s+$/g;
            return str.replace( re, '' );
        }
    };

    // remove extra white space
    Ut.normalizeString = function( str ) {
        var re = /\s\s+/g;
        return Ut.trim( str ).replace( re, ' ' );
    };
    
    Ut.hasClass = function( el, cl ) {
        var re = new RegExp( '\\b' + cl + '\\b', 'i' );
        if ( re.test(el.className) ) {
            return true;
        }
        return false;
    };

    Ut.addClass = function( el, cl ) {
        if ( !Ut.hasClass( el, cl ) ) {
            el.className = Ut.trim( el.className + ' ' + cl );
        }
    };

    Ut.removeClass = function( el, cl ) {
        el.className = Ut.normalizeString( el.className.replace(cl, ' ') );
    };
    
    // className, tagName (optional), element ref (optional)
    Ut.getElementsByClassName = function ( cl, tag, el ) {
		// use el if it's a reference to an element
        el = ( el && el.getElementsByTagName )? el: document;
        
		// use DOM methods if supported
        if ( tag && el.querySelectorAll ) {
            return el.querySelectorAll( tag + '.' + cl );
        } else if ( el.getElementsByClassName ) {
            return el.getElementsByClassName( cl );
        } else {
            tag = tag || '*';
            var result = [],
                re = new RegExp( '\\b' + cl + '\\b', 'i' ),
                list = el.getElementsByTagName( tag );
                
            for ( var i = 0, len = list.length; i < len; i++ ) {
                if ( re.test( list[i].className ) ) {
                    result.push( list[i] );
                }
            }
            return result;
        }
    };
    
    // searches query string for value associated with name (string)
    // searches window.location or link href (optional obj argument)
    Ut.getValueFromQueryString = function ( name, obj ) {
        var pairs, set;
        obj = obj? obj: window.location; 
        
        if ( obj.search && obj.search.indexOf(name !== -1) ) {
            pairs = obj.search.slice(1).split('&'); // name/value pairs
            
            for ( var i=0, len = pairs.length; i < len; i++ ) {
                set = pairs[i].split('=');
                
                // Check each pair for match on name 
                if ( set[0] === name ) {
                    return decodeURIComponent( set[1] );
                }
            }
        }
        return '';
    };

    Ut.getURLtoHash = function( obj ) {
        obj = obj? obj: window.location; 
        var href = obj.href, 
            pt = href.indexOf('#'),
            url = (pt !== -1)? href.slice( 0, pt ): href; // remove hash from href 
        return url;
    };
    
    Ut.getHash = function( obj ) {
        obj = obj? obj: window.location; 
        var hash = '';
        if ( obj.hash ) { // some browsers say true if just #, some '' (ff)
            hash = obj.hash.slice(1);
        }
        return hash;
    };
    
    // Alternate functions are available that use DOM methods instead of document.write
    // bScreen usually undefined (not used) unless pass false, applied to media screen
    Ut.writeStyleSheet = function(file, bScreen) {
        var screen = (bScreen !== false)? '" media="screen" />\n': '"/>\n';
        document.write( '\n<link rel="stylesheet" href="' + file + screen);
    };
    
    // used to set min-height (as on dyn-web home page)
    Ut.writeStyleRule = function(rule, bScreen) {
        var screen = (bScreen !== false)? ' media="screen">': '>';
        document.write( '\n<style type="text/css"' + screen + rule + '</style>');
    };

    return Ut;

})( DYN_WEB.Util || {} );


DYN_WEB.Tabs = (function() {
    var Ut = DYN_WEB.Util,
        Ev = DYN_WEB.Event,
        Cookie = DYN_WEB.Cookie;
    
    // constructor 
    // obj holds props: id, useCookies, etc.
    function Tabs(obj) {
        this.useCookies = obj.useCookies;
        this.before_hide = obj.before_hide || function() {};
        this.on_activate = obj.on_activate || function() {};
        this.on_init = obj.on_init || function() {};
        this.current = null;
        Tabs.col[obj.id] = this;
        Tabs.init(obj.id);
    }
    
    // private var and public method to get instance instead?
    Tabs.col = {};
    
    // static methods
    Tabs.setup = function() {
        var obj, settings = [];

        if ( Tabs.hasBrowserSupport() ) {
            //for (var i=0; obj = arguments[i]; i++) {
            for ( var i = 0, len = arguments.length; i < len; i++ ) {
                obj = arguments[i];
                if ( obj.css ) {
                    Ut.writeStyleSheet(obj.css);
                }
                settings[i] = obj; // save to pass to constructor onload
            }

            //Ev.add(window, 'load', function() {
            Ev.domReady( function() {
                for ( var i = 0, len = settings.length; i < len; i++ ) {
                    new Tabs( settings[i] );
                }
            });
        }
    };

    Tabs.hasBrowserSupport = function() {
        var doc = document;
        if ( doc.getElementById && doc.getElementsByTagName && 
                typeof decodeURI !== 'undefined' && 
                (doc.addEventListener || doc.attachEvent) ) {
            return true;
        }
        return false;
    };

    Tabs.init = function(tabsetId) {
        var _this = Tabs.col[tabsetId],
            links = Tabs.getTabs(tabsetId),
            // code uses hash in tab links but tabs can also link to other pages
            // so compare current location to link's href
            loc = Ut.getURLtoHash(location),
            lnk, pg, paneId, pane, sel, dflt, cur, c, q, hash;

        q = Ut.getValueFromQueryString(tabsetId); 
        if (_this.useCookies) {
            c = Tabs.checkCookie(tabsetId);
        }
        // tab in query string or cookie?
        sel = q? q: c? c: '';

        // add ids and event handlers to links ...
        for ( var i = 0, len = links.length; i < len; i++ ) {
            lnk = links[i];
            paneId = '', pane = ''; // reset
            pg = Ut.getURLtoHash(lnk);
            hash = Ut.getHash(lnk);

            if ( lnk.hash && (loc === pg) ) { // has hash and points to current page
                paneId = hash;
                lnk.id = tabsetId + '__' + paneId; // double underscore between
            }

            // if tab associated with tabpane, set up onclick fn
            if (paneId) { 
                try {
                    pane = Ut.$(paneId);    
                } catch (err) { // showClicked fn throws
                }

                // first tab/pane is default 
                if ( !dflt ) {
                    dflt = paneId;
                }

                // if set active in markup, hold as current
                if ( pane && Ut.hasClass(lnk, 'activeTab') && Ut.hasClass( pane, 'activePane') ) {
                    _this.current = cur = paneId;
                }

                // to pass paneId, tabsetId to showClicked...
                Ev.add( lnk, 'click', function(paneId, tabsetId){
                        return  function(e){
                            Tabs.showClicked(e, paneId, tabsetId);
                        };
                    }(paneId, tabsetId) );
            }
        }

        // check if tab and pane for sel
        if ( sel && (lnk = Ut.$(tabsetId + '__' + sel) ) && (pane = Ut.$(sel) ) ) {
            if ( cur && cur !== sel ) {
                Tabs.hideCurrent(tabsetId, cur);
            }
            Ut.addClass(lnk, 'activeTab');
            Ut.addClass(pane, 'activePane');
            _this.current = sel;
        } else if ( !cur && dflt ) {
            Ut.addClass( Ut.$( tabsetId + '__' + dflt), 'activeTab' );
            Ut.addClass( Ut.$(dflt), 'activePane' );
            _this.current = dflt;
        }
        _this.on_init();
    };

    Tabs.getTabs = function(id) {
        var set = Ut.$(id), list, links;
        if ( !set ) {
            throw new Error('No element matching tabset id found.');
        }
        list = Ut.getElementsByClassName('tabnavs', 'ul',  set);
        if ( list.length === 0 ) {
            throw new Error('No tabnavs were found in the tabset.');
        }
        links = list[0].getElementsByTagName('a'); 
        return links;
    };

    Tabs.showClicked = function(e, paneId, tabsetId) {
        var tabId = tabsetId + '__' + paneId;
        var _this = Tabs.col[tabsetId];
        _this.before_hide(tabId, paneId, tabsetId);
        Tabs.hideCurrent(tabsetId, _this.current);
        Ut.addClass( Ut.$(tabId), 'activeTab');

        try {
            Ut.addClass(Ut.$(paneId), 'activePane');
        } catch (err) {
            throw new Error('showClicked can\'t find pane associated with tab. Check tab hash and pane id.');
        }

        _this.current = paneId;
        if ( _this.useCookies ) {
            Tabs.setCookie(tabsetId, paneId);
        }

        _this.on_activate(tabId, paneId, tabsetId);
        if (e) {
            Ev.DOMit(e);
            e.preventDefault();
        }
        return false;
    };

    Tabs.hideCurrent = function(tabsetId, cur) {
        var tab = Ut.$( tabsetId + '__' + cur ),
            pane = Ut.$( cur );
        Ut.removeClass( tab, 'activeTab' );
        Ut.removeClass( pane, 'activePane' );
    };

    Tabs.checkCookie = function(tabsetId) {
        var c, cookies, tab_cookies = Cookie.get('dw_Tabs');
        if ( tab_cookies ) {
            cookies = tab_cookies.split(',');
            for ( var i=0, len = cookies.length; i < len; i++ ) {
                c = cookies[i];
                if ( c.indexOf(tabsetId + ':') === 0 ) {
                    return c.slice( tabsetId.length + 1 );
                }
            }
        }
        return null;
    };

    Tabs.setCookie = function(tabsetId, paneId) {
        // format for cookies (multiple tabsets supported): dw_Tabs=tabsetId:paneId,tabsetId:paneId;
        var new_tab_cookies = '', cookies,
            tab_cookies = Cookie.get('dw_Tabs');
        if ( tab_cookies ) {
            cookies = tab_cookies.split(',');
            for ( var i=0, len = cookies.length; i < len; i++ ) {
                if ( cookies[i].indexOf(tabsetId + ':') === 0 ) {
                    cookies[i] = tabsetId + ':' + paneId; // no need to encode valid id's
                    new_tab_cookies = cookies.join(',');
                    break;
                }
            }
            if ( !new_tab_cookies ) { // if no match for this tabsetId
                new_tab_cookies = tab_cookies + ',' + tabsetId + ':' + paneId;
            }
        } else { // no dw_Tabs set yet
            new_tab_cookies = tabsetId + ':' + paneId;
        }
        Cookie.set('dw_Tabs', new_tab_cookies, null, '/');
    };
    
    return Tabs;
})(); 
