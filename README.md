# laravel-state-machine
Eloquent State Machine


Install through Composer
```
composer require bidzm/eloquent-state-machine
```

## Usage
Just add `Bidzm\StateMachine` to your eloquent model.
```
namespace App;

use Illuminate\Database\Eloquent\Model;
use Bidzm\StateMachine;

class Message extends Model
{
    use StateMachine;
    protected $fieldState = 'state'; // attribute name that use as state. Default `state`;
    protected $initialState = 'initiated'; // initial state. Default `initiated`;
    protected $transitions = [
        [
            'from' => ['initiated'], // Previous state
            'to' => 'queued', // State name
            'on' => 'queue' // Action name
        ],
        [
            'from' => ['queued'], // Previous state
            'to' => 'finished', // State name
            'on' => 'finish' // Action name
        ],
    ];
}
```

There some function that you can use
```
<?php

$message = new App\Message();
$message->save();
$message->state; // `initiated`

$message->can('queue'); // `true`
// `canQueued` method on your model where `Queued` is the "studly" cased name of the state you want check
$message->canQueued(); // `true`
$message->canFinished(); // `false`

$message->isState('initiated'); // `true`
// `isInitiated` method on your model where `Initiated` is the "studly" cased name of the state you want check
$message->isInitiated(); // `true`
$message->isFinished(); // `false`

$message->queue();
$message->state; // `queued`
```

If you want doing some action in after or before transition fired. You must override method `beforeTransition()` or `afterTransition()`
```
<?php

public function beforeTransition($from, $to)
{
    // do something
}

public function afterTransition($from, $to)
{
    // do something
}
```

## State Change At
You can store when state changed, with attribute `state_change_at` in your table
```
namespace App;

use Illuminate\Database\Eloquent\Model;
use Bidzm\StateMachine;

class Message extends Model
{
    use StateMachine;
    protected $fieldState = 'state'; // Attribute name that use as state. Default `state`;
    protected $initialState = 'initiated'; // Initial state. Default `initiated`;
    protected $useChangeAt = true; // Determine if store time when state changed. Default false;
    protected $transitions = [
        [
            'from' => ['initiated'], // Previous state
            'to' => 'queued', // State name
            'on' => 'queue' // Name action
        ],
    ];
}
```
There some function that you can use
```
<?php

$message = new App\Message();
$message->save();
$message->state; // `initiated`
$message->initiatedAt(); // 2017-08-19T21:07:00+07:00

$message->queue();
$message->state; // `queued`
$message->initiatedAt(); // 2017-08-19T21:07:00+07:00
$message->queuedAt(); // 2017-08-19T21:07:01+07:00
```

## License
MIT - http://opensource.org/licenses/MIT
