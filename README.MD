## Architecture Overview
This is a React-based video conferencing application built on AWS Chime SDK. It follows a client-server architecture:

Frontend: React SPA with TypeScript
Backend: Express.js server that interfaces with AWS Chime SDK
Infrastructure: Optional serverless deployment using AWS Lambda/API Gateway

## Directory Structure
### Root Level
`server.js` - Express server that handles meeting creation/management via AWS Chime SDK
`webpack.config.js` - Webpack configuration for development and production builds
`package.json` - Project dependencies and scripts
`tsconfig.json` - TypeScript configuration

### `/src` - Main Application Code
#### `/src/views` - Page Components
`Home/` - Landing page with meeting join form
`DeviceSetup/` - Audio/video device configuration before joining
`Meeting/` - Main meeting interface with video grid and controls
#### `/src/containers` - Smart Components
`MeetingForm/` - Meeting creation/join form logic
`MeetingControls/` - Meeting control bar (mute, video, screen share, etc.)
`MeetingProviderWrapper/` - Main context provider wrapper
`Navigation/` - Side navigation (roster, chat)
`Chat/` - In-meeting chat functionality
`DevicePermissionControl/` - Browser permission handling
#### `/src/components` - Presentational Components
`VideoTileGrid/` - Video grid layout for participants
`DeviceSelection/` - Device picker components
`Card/`, `ActivityBar/` - UI components
`icons/` - Custom icon components
#### `/src/providers` - Context Providers
`AppStateProvider` - Global app state management
`NavigationProvider` - Navigation state (sidebar visibility)
`VideoTileGridProvider` - Video grid state management
`DataMessagesProvider` - Real-time messaging
`ErrorProvider` - Error handling context
`SIPMeetingProvider` - SIP meeting support
#### `/src/utils` - Utilities
`api.ts` - API calls to backend server
`background-replacement.ts` - Video background effects
Various helper functions
#### `/serverless` - AWS Deployment
CloudFormation templates
Lambda function handlers
Deployment scripts
#### `/app` - Static Assets
`meeting.html` - Meeting page template

## To run the demo app locally:

1. If you haven't already, [configure your AWS credentials](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html).

2. If cloning and running for the first time skip this step. If you want to make demo changes based on latest commits in `amazon-chime-sdk-component-library-react` (already added as a submodule to this repository), then, update submodule to the latest commit using:
```
// from root of this cloned repository
git submodule update --remote amazon-chime-sdk-component-library-react
```

3. In the root directory `cd apps/meeting`, run `npm install`

4. Start the webpack server and node server concurrently: `npm start`

5. Open https://0.0.0.0:9000/ in your browser

### Troubleshooting

- You can change the `amazon-chime-sdk-js` [log level](https://aws.github.io/amazon-chime-sdk-js/enums/loglevel.html) by providing `logLevel` through URL as a query parameter with value 'warn' | 'error' | 'info' | 'debug' | 'off'.

  ```
   https://0.0.0.0:9000/?logLevel=debug
  ```

- `UnrecognizedClientException: The security token included in the request is invalid.` when trying to join a meeting

  This likely means that your AWS credentials are invalid. Refer to the [AWS Configuration docs](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html) for setting up your credentials.

## To deploy the serverless device demo

### Install aws and sam command line tools

- [Install the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv1.html)
- [Install the AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)

### Run deployment script

The following will create a CloudFormation stack containing a Lambda and API Gateway deployment that runs the demo. Note that S3 bucket name should be globally unique.

```
cd /serverless
node ./deploy.js -r us-east-1 -b <YOUR-S3-BUCKET-NAME> -s <YOUR-CLOUDFORMATION-STACK-NAME>
```

The script will create an S3 bucket and CloudFormation stack with Lambda and API Gateway resources required to run the demo. After the script finishes, it will output a URL that can be opened in a browser.
