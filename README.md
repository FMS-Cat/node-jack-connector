Node.JS JACK-connector
======================

Bindings JACK-Audio-Connection-Kit for Node.JS

🎉🎉🎉 **Now it's (probably) working on node 10.x!!**　🎉🎉🎉  
I'm afraid of memory leak, I'm pretty n00b at native coding 😖

Install
=======
```bash
npm install fms-cat/node-jack-connector#master
```

Build requirements
==================
libjack2, libjack2-devel

How to use
==========
```javascript
var jackConnector = require('jack-connector');
jackConnector.openClientSync('Noize Generator');
jackConnector.registerOutPortSync('output');

function audioProcess(err, nframes) {
	if (err) {
		console.error(err);
		process.exit(1);
		return;
	}

	var ret = [];
	for (var i=0; i<nframes; i++) ret.push((Math.random() * 2) - 1);
	return { output: ret };
}

jackConnector.bindProcessSync(audioProcess);
jackConnector.activateSync();
jackConnector.connectPortSync('Noize Generator:output', 'system:playback_1');
jackConnector.connectPortSync('Noize Generator:output', 'system:playback_2');

(function mainLoop() { setTimeout(mainLoop, 1000000000); })();

process.on('SIGTERM', function () {
	jackConnector.deactivateSync();
	jackConnector.closeClient(function (err) {
		if (err) {
			console.error(err);
			process.exit(1);
			return;
		}

		process.exit(0);
	});
});
```

More examples
=============

[examples/](./examples/)

License
=======

MIT
