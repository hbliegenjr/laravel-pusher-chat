# laravel-pusher-chat
CMSC 207

	LARAVEL PUSHER INSTALLATION, CONFIGURATION & CHAT IMPLEMENTATION
	1. Sign-up/Sign-in to pusher.com and create channel app
	• Front end: Vanilla JS
	• Back end: Laravel
	2. Copy App Keys
	3. Install "Composer"
	4. Install "Npm"
	5. Install Pusher
	composer require pusher/pusher-php-server "~7.0"
	6. Create Project
	composer create-project laravel/laravel "name"
	7. Change directory
	cd "name"
	8. Run npm install
	npm install
	9. Open in folder in Vscode
	code .
	10. Open .env file, paste App Keys and edit broadcast driver
	PUSHER_APP_ID= 
	PUSHER_APP_KEY= 
	PUSHER_APP_SECRET=
	PUSHER_APP_CLUSTER= 
	……..
	BROADCAST_DRIVER=pusher

	11. Create event (e.g. Message)
	php artisan make:event Message
	12. Open and Edit event 'Message'
	
	class Message implements ShouldBroadcast {
	    use Dispatchable, InteractsWithSockets, SerializesModels;
	    public $username;
	    public $message;
	    /**
	     * Create a new event instance.
	     *
	     * @return void
	     */
	    public function __construct($username, $message) {
	        $this->username = $username;
	        $this->message = $message;
	    }
	    /**
	     * Get the channels the event should broadcast on.
	     *
	     * @return \Illuminate\Broadcasting\Channel|array
	     */
	    public function broadcastOn()
	    {
	        return new Channel('chat');
	    }
	    public function broadcastAs() {
	        return 'message';
	        
	    }
	}
	
	13. Open and edit routes/web.php
	<?php
	use Illuminate\Support\Facades\Route;
	use App\Events\Message;
	use Illuminate\Http\Request;
	use Illuminate\Http\Response;
	/*
	|--------------------------------------------------------------------------
	| Web Routes
	|--------------------------------------------------------------------------
	|
	| Here is where you can register web routes for your application. These
	| routes are loaded by the RouteServiceProvider within a group which
	| contains the "web" middleware group. Now create something great!
	|
	*/
	
	
	Route::get('/', function () {
	    return view('index');
	});
	Route::post('/send-message', function (Request $request) {
	    event (
	        new Message(
	            $request->input('username'), 
	            $request->input('message')
	        )
	    );
	    return ["success" => true] ;
	});
	
	14. Open and edit resources/views/index.blade.php (renamed)
	<!DOCTYPE html>
	<html lang="en">
	    <head>
	        <meta charset="utf-8">
	        <meta http-equiv="X-UA-Compatible" content="IE=edge">
	        <meta name = "viewport" content = "width=device-width,
	        initial-scale=1.0">
	        <title>Laravel-Pusher Chat</title>
	 
	    </head>
	    <body>
	        <h1> HELLO WORLD!</h1>
	    </body>
	</html>
	
	15. Run server
	php artisan serve
	16. Edit index.blade.php
	
	<!DOCTYPE html>
	<html lang="en">
	    <head>
	        <meta charset="utf-8">
	        <meta http-equiv="X-UA-Compatible" content="IE=edge">
	        <meta name = "viewport" content = "width=device-width,
	        initial-scale=1.0">
	        <title>Laravel-Pusher Chat</title>
	        <link rel="stylesheet" href="./css/app.css" />
	    </head>
	    <body>
	    <div class="app">
	        <header>
	            <h1>Let's Talk!</h1>
	            <input type="text" name="username" id="username" placeholder="Please enter
	            a username..."/>
	        </header>
	        <div id="messages"></div>
	            <form id="message_form">
	            <input type="text" name="message" id="message_input" placeholder="Write a
	            message..." />
	<button type="submit" id="message_send">Send Message</button>
	</form>
	</div>
	<script src="./js/app.js"> </script>
	    </body>
	</html>
	
	17. Edit resources/css/app.css
	
	* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
	font-family: 'Bicyclette', sans-serif;
	}
	body {
	background-color:#EEE;
	}
	input, button {
	appearance: none;
	border: none;
	outline: none;
	background: none;
	}
	input {
	display: block;
	width: 100%;
	background-color:#FFF;
	padding: 12px 16px;
	font-size: 18px;
	color: #888;
	}
	.app {
	    display: flex;
	    flex-direction:column;
	    height: 100vh;
	}
	    
	header {
	    display: flex;
	    padding-top: 128px;
	    padding-bottom: 32px;
	    background-color:#8C38FF;
	    background-image: linear-gradient(to bottom, #8C38FF,
	    #6317ce);
	    align-items: center;
	    justify-content: flex-end;
	    flex-direction: column;
	    box-shadow: Opx 6px 12px Irgba(0, 0, 0, 0.15);
	    padding-left: 16px;
	    padding-right: 16px;
	    }
	h1 {
	color: #FFF;
	text-transform: uppercase;
	text-align: center;
	margin-bottom: 16px;
	}
	#username {
	border-radius: 8px;
	transition: 0.4s;
	text-align: center;
	}
	#username:focus {
	box-shadow: 0px 6px 12px rgba(0, 0, 0, 0.25) ;
	}
	#messages {
	flex: 1 1 0%;
	overflow: scroll;
	padding: 16px;
	}
	.message {
	    display: block;
	    width: 100%;
	    border-radius: 99px;
	    background-color:#FFF;
	    padding: 8px 16px;
	    box-shadow: 0px 6px 12px rgba(0, 0, 0, 0.15);
	    font-weight: 400;
	    margin-bottom: 16px;
	}
	.message strong {
	    color: #8C38FF;
	    font-weight: 600;
	}
	#message_form {
	    display: flex;
	}
	#message_input {
	    flex: 1 1 0%;
	}
	#message_send {
	    appearance: none;
	    background-color:#8C38FF;
	    padding: 4px 8px;
	    color: #FFF;
	    text-transform: uppercase;
	}
	
	18. Edit js/bootstrap.js and uncomment laravel echo
	19. Install laravel echo and pusher
		npm i --save laravel-echo pusher-js 
	20. Edit js/app.js 

require('./bootstrap');
const messages_el = document.getElementById ("messages") ;
const username_input = document.getElementById ("username") ;
const message_input = document.getElementById ("message_input") ;
const message_form = document.getElementById ("message_form") ;
message_form.addEventListener('submit', function (e) {
    e.preventDefault();
    let has_errors = false;
    if (username_input.value == '') {
        alert ("Please enter a username") ;
        has_errors = true;
    }
    if (message_input.value == '') {
        alert ("Please enter a message");
        has_errors = true;
    }
    if (has_errors) {
        return;
    }
    
    const options = {
        method: 'post',
        url: '/send-message',
        data: {
            username: username_input.value,
            message: message_input.value
        }
    }
    axios(options);
});
window.Echo.channel('chat')
    .listen('.message', (e) => {
        messages_el.innerHTML += '<div class = "message"><strong>' + e.username + ':</strong> ' + e.message + '</div>';
    });
	21. Compile/mix changes to JS
	npm run watch
	22. Refresh URL and start chatting
![image](https://user-images.githubusercontent.com/91530882/163904422-42e5c203-2468-4bb7-9681-c3e0bb8f2d71.png)

