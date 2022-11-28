![Build Health](https://github.com/mpaauw/yinston/actions/workflows/build.yml/badge.svg)

# yinston (wip)
A light, easy-to-use, production-ready wrapper for [Winston](https://www.npmjs.com/package/winston).

## Installation
To get started, simply install via npm:

```shell
npm install @mpaauw/yinston
```

## Usage
To use Yinston for logging, first instantiate your logger, passing in an optional filename to be used by the logger itself:

### Instantiation
```typescript
import { Logger } from 'winston';
import { YinstonLogger } from '@mpaauw/yinston';

const logger: Logger = YinstonLogger.createLogger(__filename);
```

### Logging Events
Next, you can use your logger as you normally would, passing events with the relevant severity level:

```typescript
logger.info('Not all those who wander are lost.');

logger.debug('All we have to decide is what to do with the time that is given us.');

logger.error('It’s the job that’s never started as takes longest to finish.');
```

... which should give you the following logs:

```
[2022-11-28T20:55:30.039Z] [yourFileNameHere.ts] info: Not all those who wander are lost.

[2022-11-28T20:55:30.039Z] [yourFileNameHere.ts] debug: All we have to decide is what to do with the time that is given us.

[2022-11-28T20:55:30.040Z] [yourFileNameHere.ts] error: Its the job thats never started as takes longest to finish.
```

### Logging Objects
You can also easily pass entire objects to be written to your log event:

```typescript
logger.debug(`We've had one, yes. What about second breakfast?`, {
    character: {
      name: 'Peregrin Took',
      heightInMeters: 1.38,
      realm: Locations.Shire
    },
    secondBreakfast: {
      isHappening: false
    }
  });
```

which results in the following log:

```
[2022-11-28T21:02:03.441Z] [yourFileNameHere.ts] debug: We've had one, yes. What about second breakfast?
{
  character: { name: 'Peregrin Took', heightInMeters: 1.38, realm: 'Shire' },
  secondBreakfast: { isHappening: false }
}
```

### Configuration
It's important to note that by default, Yinston will pretty-print logs (objects included). To disable this and opt for json-only logging (which is recommended for production environments), simply set the following variable in your environment:

```
NODE_ENV=production
```

This will instruct Yinston to only print logs to .json format. For example, the object log described above, when `NODE_ENV` is set to `production`, would look like this:

```
{"character":{"heightInMeters":1.38,"name":"Peregrin Took","realm":"Shire"},"level":"debug","message":"We've had one, yes. What about second breakfast?","secondBreakfast":{"isHappening":false},"timestamp":"2022-11-28T21:21:00.933Z"}
```

### Testing
To mute logs when running tests, simply include a `--silent` within your test script. 

For example, if you're running unit tests with Jest, simply add the following test script in your `package.json` file:

```
"test": "jest --runInBand --coverage --silent --forceExit --testPathPattern=*",
```
