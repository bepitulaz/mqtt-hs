mqtt-hs
=======

A Haskell MQTT client library.

A simple example, assuming a broker is running on localhost (needs -XOverloadedStrings):

```haskell
{-# Language OverloadedStrings #-}
import Network.MQTT
import Network.MQTT.Logger
import GHC.Conc.Sync
import Control.Concurrent.STM

main = do
  mqtt <- connect defaultConfig { cLogger = warnings stdLogger }

  -- Publish to the topic. 
  publish mqtt Handshake False "random/topic" "this is the content"
  
  -- Subscribe to the topic.
  let f (t, payload) = "A message was published to " ++ show t ++ ": " ++ show payload
  subscribe mqtt NoConfirm "#" 
  f <$> atomically (readTChan (messages mqtt))
```

See the haddock for more documentation.
