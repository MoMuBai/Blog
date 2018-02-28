# DLNA
## SSDP 发现设备
### 搜索消息
```
M-SEARCH * HTTP/1.1
MAN: "ssdp:discover"
MX: 5
HOST: 239.255.255.250:1900
ST: urn:schemas-upnp-org:service:AVTransport:1
```
### 响应
```
HTTP/1.1 200 OK
Location: http://10.0.206.238:1708/
Cache-Control: max-age=1800
Server: UPnP/1.0 DLNADOC/1.50 Platinum/1.0.5.13
EXT: 
BOOTID.UPNP.ORG: 0
CONFIGID.UPNP.ORG: 5250633
USN: uuid:2e7b079b-8f96-266b-537e-b8f445ee2a6f::urn:schemas-upnp-org:service:AVTransport:1
ST: urn:schemas-upnp-org:service:AVTransport:1
Date: Mon, 26 Feb 2018 06:38:32 GMT
```

### Swift 代码示例

```swift
import UIKit
import CocoaAsyncSocket
import AEXML

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
        
        let host: String = "239.255.255.250"
        let port: UInt16 = 1900
        let udpSocket = GCDAsyncUdpSocket.init(delegate: self as GCDAsyncUdpSocketDelegate, delegateQueue: DispatchQueue.global(qos: DispatchQoS.QoSClass.default))
        do {
            try udpSocket.bind(toPort: port)
        } catch {
            print("error in bindToPort")
        }
        let dataString: String = "M-SEARCH * HTTP/1.1\r\nMAN: \"ssdp:discover\"\r\nMX: 5\nHOST: 239.255.255.250:1900\r\nST: urn:schemas-upnp-org:service:AVTransport:1\r\n\r\n"
        let data: Data = dataString.data(using: .utf8)!
        udpSocket.send(data, toHost: host, port: port, withTimeout: 100, tag: 100)
        try! udpSocket.beginReceiving()
        
        
        
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
}

extension ViewController: GCDAsyncUdpSocketDelegate {
    
    func udpSocket(_ sock: GCDAsyncUdpSocket, didConnectToAddress address: Data) {
        print("didConnectToAddress")
    }
    
    func udpSocket(_ sock: GCDAsyncUdpSocket, didNotConnect error: Error?) {
        print("didNotConnect");
        print("error:\(error?.localizedDescription ?? "error")")
    }
    
    func udpSocket(_ sock: GCDAsyncUdpSocket, didSendDataWithTag tag: Int) {
        print("didSendDataWithTag")
    }
    
    func udpSocket(_ sock: GCDAsyncUdpSocket, didNotSendDataWithTag tag: Int, dueToError error: Error?) {
        print("didNotSendDataWithTag")
        print("error:\(error?.localizedDescription ?? "error")")
    }
    
    
    
    func udpSocket(_ sock: GCDAsyncUdpSocket, didReceive data: Data, fromAddress address: Data, withFilterContext filterContext: Any?) {
        let msg: String = String(data: data, encoding: .utf8)!
        print("-------------- didReceiveData start ---------------");
        print("msg:\(msg)")
        print("address:\(address)")
        print("filterContext:\(filterContext ?? "")")
        print("-------------- didReceiveData end ---------------")
    }
    
    func udpSocketDidClose(_ sock: GCDAsyncUdpSocket, withError error: Error?) {
        print("udpSocketDidClose")
        print("error:\(error?.localizedDescription ?? "error")")
    }
}
```

## 获取设备描述文档

根据 Location 地址获取设备描述。如上 http://10.0.206.238:1708/

```xml

<?xml version="1.0" encoding="UTF-8"?>
<root configId="14344912" xmlns="urn:schemas-upnp-org:device-1-0" xmlns:dlna="urn:schemas-dlna-org:device-1-0">
  <specVersion>
    <major>1</major>
    <minor>1</minor>
  </specVersion>
  <device>
    <deviceType>urn:schemas-upnp-org:device:MediaRenderer:1</deviceType>
    <friendlyName>Kodi (KevinRMBP15.local)</friendlyName>
    <manufacturer>XBMC Foundation</manufacturer>
    <manufacturerURL>http://kodi.tv/</manufacturerURL>
    <modelDescription>Kodi - Media Renderer</modelDescription>
    <modelName>Kodi</modelName>
    <modelNumber>17.6 Git:20171114-a9a7a20</modelNumber>
    <modelURL>http://kodi.tv/</modelURL>
    <UDN>uuid:2e7b079b-8f96-266b-537e-b8f445ee2a6f</UDN>
    <presentationURL>http://10.0.206.238:8080/</presentationURL>
    <dlna:X_DLNADOC xmlns:dlna="urn:schemas-dlna-org:device-1-0">DMR-1.50</dlna:X_DLNADOC>
    <iconList>
      <icon>
        <mimetype>image/png</mimetype>
        <width>256</width>
        <height>256</height>
        <depth>8</depth>
        <url>/icon256x256.png</url>
      </icon>
      <icon>
        <mimetype>image/png</mimetype>
        <width>120</width>
        <height>120</height>
        <depth>8</depth>
        <url>/icon120x120.png</url>
      </icon>
      <icon>
        <mimetype>image/png</mimetype>
        <width>48</width>
        <height>48</height>
        <depth>8</depth>
        <url>/icon48x48.png</url>
      </icon>
      <icon>
        <mimetype>image/png</mimetype>
        <width>32</width>
        <height>32</height>
        <depth>8</depth>
        <url>/icon32x32.png</url>
      </icon>
      <icon>
        <mimetype>image/png</mimetype>
        <width>16</width>
        <height>16</height>
        <depth>8</depth>
        <url>/icon16x16.png</url>
      </icon>
    </iconList>
    <serviceList>
      <service>
        <serviceType>urn:schemas-upnp-org:service:AVTransport:1</serviceType>
        <serviceId>urn:upnp-org:serviceId:AVTransport</serviceId>
        <SCPDURL>/AVTransport/2e7b079b-8f96-266b-537e-b8f445ee2a6f/scpd.xml</SCPDURL>
        <controlURL>/AVTransport/2e7b079b-8f96-266b-537e-b8f445ee2a6f/control.xml</controlURL>
        <eventSubURL>/AVTransport/2e7b079b-8f96-266b-537e-b8f445ee2a6f/event.xml</eventSubURL>
      </service>
      <service>
        <serviceType>urn:schemas-upnp-org:service:ConnectionManager:1</serviceType>
        <serviceId>urn:upnp-org:serviceId:ConnectionManager</serviceId>
        <SCPDURL>/ConnectionManager/2e7b079b-8f96-266b-537e-b8f445ee2a6f/scpd.xml</SCPDURL>
        <controlURL>/ConnectionManager/2e7b079b-8f96-266b-537e-b8f445ee2a6f/control.xml</controlURL>
        <eventSubURL>/ConnectionManager/2e7b079b-8f96-266b-537e-b8f445ee2a6f/event.xml</eventSubURL>
      </service>
      <service>
        <serviceType>urn:schemas-upnp-org:service:RenderingControl:1</serviceType>
        <serviceId>urn:upnp-org:serviceId:RenderingControl</serviceId>
        <SCPDURL>/RenderingControl/2e7b079b-8f96-266b-537e-b8f445ee2a6f/scpd.xml</SCPDURL>
        <controlURL>/RenderingControl/2e7b079b-8f96-266b-537e-b8f445ee2a6f/control.xml</controlURL>
        <eventSubURL>/RenderingControl/2e7b079b-8f96-266b-537e-b8f445ee2a6f/event.xml</eventSubURL>
      </service>
    </serviceList>
  </device>
</root>
```

主要有设备信息 \<device\> 和支持的服务列表 \<serviceList\>

```xml
<service>
    <serviceType>urn:schemas-upnp-org:service:AVTransport:1</serviceType>
    <serviceId>urn:upnp-org:serviceId:AVTransport</serviceId>
    <SCPDURL>/AVTransport/2e7b079b-8f96-266b-537e-b8f445ee2a6f/scpd.xml</SCPDURL>
    <controlURL>/AVTransport/2e7b079b-8f96-266b-537e-b8f445ee2a6f/control.xml</controlURL>
    <eventSubURL>/AVTransport/2e7b079b-8f96-266b-537e-b8f445ee2a6f/event.xml</eventSubURL>
</service>
```

* serviceId : 必有字段。服务表示符，是服务实例的唯一标识。
* serviceType : 必有字段。UPnP服务类型。格式定义与deviceType类此。详看文章开头表格。
* SCPDURL : 必有字段。Service Control Protocol Description URL，获取设备描述文档URL。
* controlURL : 必有字段。向服务发出控制消息的URL，详见 基于DLNA实现iOS，Android投屏：SOAP控制设备
* eventSubURL : 必有字段。订阅该服务时间的URL，详见 基于DLNA实现iOS，Android投屏：SOAP控制设备

## 获取服务描述文档
服务描述文档是对服务功能的基本说明，包括服务上的动作及参数，还有状态变量和其数据类型、取值范围等。通过上面 SCPDURL 拼接获取服务描述文档，如上为 http://10.0.206.238:1708/AVTransport/2e7b079b-8f96-266b-537e-b8f445ee2a6f/scpd.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<scpd xmlns="urn:schemas-upnp-org:service-1-0">
  <specVersion>
    <major>1</major>
    <minor>0</minor>
  </specVersion>
  <actionList>
    <action>
      <name>GetCurrentTransportActions</name>
      <argumentList>
        <argument>
          <name>InstanceID</name>
          <direction>in</direction>
          <relatedStateVariable>A_ARG_TYPE_InstanceID</relatedStateVariable>
        </argument>
        <argument>
          <name>Actions</name>
          <direction>out</direction>
          <relatedStateVariable>CurrentTransportActions</relatedStateVariable>
        </argument>
      </argumentList>
    </action>
    <action>
      <name>GetDeviceCapabilities</name>
      <argumentList>
        <argument>
          <name>InstanceID</name>
          <direction>in</direction>
          <relatedStateVariable>A_ARG_TYPE_InstanceID</relatedStateVariable>
        </argument>
        <argument>
          <name>PlayMedia</name>
          <direction>out</direction>
          <relatedStateVariable>PossiblePlaybackStorageMedia</relatedStateVariable>
        </argument>
        <argument>
          <name>RecMedia</name>
          <direction>out</direction>
          <relatedStateVariable>PossibleRecordStorageMedia</relatedStateVariable>
        </argument>
        <argument>
          <name>RecQualityModes</name>
          <direction>out</direction>
          <relatedStateVariable>PossibleRecordQualityModes</relatedStateVariable>
        </argument>
      </argumentList>
    </action>
    <action>
      <name>GetMediaInfo</name>
      <argumentList>
        <argument>
          <name>InstanceID</name>
          <direction>in</direction>
          <relatedStateVariable>A_ARG_TYPE_InstanceID</relatedStateVariable>
        </argument>
        <argument>
          <name>NrTracks</name>
          <direction>out</direction>
          <relatedStateVariable>NumberOfTracks</relatedStateVariable>
        </argument>
        <argument>
          <name>MediaDuration</name>
          <direction>out</direction>
          <relatedStateVariable>CurrentMediaDuration</relatedStateVariable>
        </argument>
        <argument>
          <name>CurrentURI</name>
          <direction>out</direction>
          <relatedStateVariable>AVTransportURI</relatedStateVariable>
        </argument>
        <argument>
          <name>CurrentURIMetaData</name>
          <direction>out</direction>
          <relatedStateVariable>AVTransportURIMetaData</relatedStateVariable>
        </argument>
        <argument>
          <name>NextURI</name>
          <direction>out</direction>
          <relatedStateVariable>NextAVTransportURI</relatedStateVariable>
        </argument>
        <argument>
          <name>NextURIMetaData</name>
          <direction>out</direction>
          <relatedStateVariable>NextAVTransportURIMetaData</relatedStateVariable>
        </argument>
        <argument>
          <name>PlayMedium</name>
          <direction>out</direction>
          <relatedStateVariable>PlaybackStorageMedium</relatedStateVariable>
        </argument>
        <argument>
          <name>RecordMedium</name>
          <direction>out</direction>
          <relatedStateVariable>RecordStorageMedium</relatedStateVariable>
        </argument>
        <argument>
          <name>WriteStatus</name>
          <direction>out</direction>
          <relatedStateVariable>RecordMediumWriteStatus</relatedStateVariable>
        </argument>
      </argumentList>
    </action>
    <action>
      <name>GetPositionInfo</name>
      <argumentList>
        <argument>
          <name>InstanceID</name>
          <direction>in</direction>
          <relatedStateVariable>A_ARG_TYPE_InstanceID</relatedStateVariable>
        </argument>
        <argument>
          <name>Track</name>
          <direction>out</direction>
          <relatedStateVariable>CurrentTrack</relatedStateVariable>
        </argument>
        <argument>
          <name>TrackDuration</name>
          <direction>out</direction>
          <relatedStateVariable>CurrentTrackDuration</relatedStateVariable>
        </argument>
        <argument>
          <name>TrackMetaData</name>
          <direction>out</direction>
          <relatedStateVariable>CurrentTrackMetaData</relatedStateVariable>
        </argument>
        <argument>
          <name>TrackURI</name>
          <direction>out</direction>
          <relatedStateVariable>CurrentTrackURI</relatedStateVariable>
        </argument>
        <argument>
          <name>RelTime</name>
          <direction>out</direction>
          <relatedStateVariable>RelativeTimePosition</relatedStateVariable>
        </argument>
        <argument>
          <name>AbsTime</name>
          <direction>out</direction>
          <relatedStateVariable>AbsoluteTimePosition</relatedStateVariable>
        </argument>
        <argument>
          <name>RelCount</name>
          <direction>out</direction>
          <relatedStateVariable>RelativeCounterPosition</relatedStateVariable>
        </argument>
        <argument>
          <name>AbsCount</name>
          <direction>out</direction>
          <relatedStateVariable>AbsoluteCounterPosition</relatedStateVariable>
        </argument>
      </argumentList>
    </action>
    <action>
      <name>GetTransportInfo</name>
      <argumentList>
        <argument>
          <name>InstanceID</name>
          <direction>in</direction>
          <relatedStateVariable>A_ARG_TYPE_InstanceID</relatedStateVariable>
        </argument>
        <argument>
          <name>CurrentTransportState</name>
          <direction>out</direction>
          <relatedStateVariable>TransportState</relatedStateVariable>
        </argument>
        <argument>
          <name>CurrentTransportStatus</name>
          <direction>out</direction>
          <relatedStateVariable>TransportStatus</relatedStateVariable>
        </argument>
        <argument>
          <name>CurrentSpeed</name>
          <direction>out</direction>
          <relatedStateVariable>TransportPlaySpeed</relatedStateVariable>
        </argument>
      </argumentList>
    </action>
    <action>
      <name>GetTransportSettings</name>
      <argumentList>
        <argument>
          <name>InstanceID</name>
          <direction>in</direction>
          <relatedStateVariable>A_ARG_TYPE_InstanceID</relatedStateVariable>
        </argument>
        <argument>
          <name>PlayMode</name>
          <direction>out</direction>
          <relatedStateVariable>CurrentPlayMode</relatedStateVariable>
        </argument>
        <argument>
          <name>RecQualityMode</name>
          <direction>out</direction>
          <relatedStateVariable>CurrentRecordQualityMode</relatedStateVariable>
        </argument>
      </argumentList>
    </action>
    <action>
      <name>Next</name>
      <argumentList>
        <argument>
          <name>InstanceID</name>
          <direction>in</direction>
          <relatedStateVariable>A_ARG_TYPE_InstanceID</relatedStateVariable>
        </argument>
      </argumentList>
    </action>
    <action>
      <name>Pause</name>
      <argumentList>
        <argument>
          <name>InstanceID</name>
          <direction>in</direction>
          <relatedStateVariable>A_ARG_TYPE_InstanceID</relatedStateVariable>
        </argument>
      </argumentList>
    </action>
    <action>
      <name>Play</name>
      <argumentList>
        <argument>
          <name>InstanceID</name>
          <direction>in</direction>
          <relatedStateVariable>A_ARG_TYPE_InstanceID</relatedStateVariable>
        </argument>
        <argument>
          <name>Speed</name>
          <direction>in</direction>
          <relatedStateVariable>TransportPlaySpeed</relatedStateVariable>
        </argument>
      </argumentList>
    </action>
    <action>
      <name>Previous</name>
      <argumentList>
        <argument>
          <name>InstanceID</name>
          <direction>in</direction>
          <relatedStateVariable>A_ARG_TYPE_InstanceID</relatedStateVariable>
        </argument>
      </argumentList>
    </action>
    <action>
      <name>Seek</name>
      <argumentList>
        <argument>
          <name>InstanceID</name>
          <direction>in</direction>
          <relatedStateVariable>A_ARG_TYPE_InstanceID</relatedStateVariable>
        </argument>
        <argument>
          <name>Unit</name>
          <direction>in</direction>
          <relatedStateVariable>A_ARG_TYPE_SeekMode</relatedStateVariable>
        </argument>
        <argument>
          <name>Target</name>
          <direction>in</direction>
          <relatedStateVariable>A_ARG_TYPE_SeekTarget</relatedStateVariable>
        </argument>
      </argumentList>
    </action>
    <action>
      <name>SetAVTransportURI</name>
      <argumentList>
        <argument>
          <name>InstanceID</name>
          <direction>in</direction>
          <relatedStateVariable>A_ARG_TYPE_InstanceID</relatedStateVariable>
        </argument>
        <argument>
          <name>CurrentURI</name>
          <direction>in</direction>
          <relatedStateVariable>AVTransportURI</relatedStateVariable>
        </argument>
        <argument>
          <name>CurrentURIMetaData</name>
          <direction>in</direction>
          <relatedStateVariable>AVTransportURIMetaData</relatedStateVariable>
        </argument>
      </argumentList>
    </action>
    <action>
      <name>SetNextAVTransportURI</name>
      <argumentList>
        <argument>
          <name>InstanceID</name>
          <direction>in</direction>
          <relatedStateVariable>A_ARG_TYPE_InstanceID</relatedStateVariable>
        </argument>
        <argument>
          <name>NextURI</name>
          <direction>in</direction>
          <relatedStateVariable>NextAVTransportURI</relatedStateVariable>
        </argument>
        <argument>
          <name>NextURIMetaData</name>
          <direction>in</direction>
          <relatedStateVariable>NextAVTransportURIMetaData</relatedStateVariable>
        </argument>
      </argumentList>
    </action>
    <action>
      <name>SetPlayMode</name>
      <argumentList>
        <argument>
          <name>InstanceID</name>
          <direction>in</direction>
          <relatedStateVariable>A_ARG_TYPE_InstanceID</relatedStateVariable>
        </argument>
        <argument>
          <name>NewPlayMode</name>
          <direction>in</direction>
          <relatedStateVariable>CurrentPlayMode</relatedStateVariable>
        </argument>
      </argumentList>
    </action>
    <action>
      <name>Stop</name>
      <argumentList>
        <argument>
          <name>InstanceID</name>
          <direction>in</direction>
          <relatedStateVariable>A_ARG_TYPE_InstanceID</relatedStateVariable>
        </argument>
      </argumentList>
    </action>
  </actionList>
  <serviceStateTable>
    <stateVariable sendEvents="no">
      <name>CurrentPlayMode</name>
      <dataType>string</dataType>
      <defaultValue>NORMAL</defaultValue>
      <allowedValueList>
        <allowedValue>NORMAL</allowedValue>
        <allowedValue>REPEAT_ONE</allowedValue>
        <allowedValue>REPEAT_ALL</allowedValue>
        <allowedValue>SHUFFLE</allowedValue>
        <allowedValue>SHUFFLE_NOREPEAT</allowedValue>
      </allowedValueList>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>RecordStorageMedium</name>
      <dataType>string</dataType>
      <allowedValueList>
        <allowedValue>NOT_IMPLEMENTED</allowedValue>
      </allowedValueList>
    </stateVariable>
    <stateVariable sendEvents="yes">
      <name>LastChange</name>
      <dataType>string</dataType>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>RelativeTimePosition</name>
      <dataType>string</dataType>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>CurrentTrackURI</name>
      <dataType>string</dataType>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>CurrentTrackDuration</name>
      <dataType>string</dataType>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>CurrentRecordQualityMode</name>
      <dataType>string</dataType>
      <allowedValueList>
        <allowedValue>NOT_IMPLEMENTED</allowedValue>
      </allowedValueList>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>CurrentMediaDuration</name>
      <dataType>string</dataType>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>AbsoluteCounterPosition</name>
      <dataType>i4</dataType>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>RelativeCounterPosition</name>
      <dataType>i4</dataType>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>A_ARG_TYPE_InstanceID</name>
      <dataType>ui4</dataType>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>AVTransportURI</name>
      <dataType>string</dataType>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>TransportState</name>
      <dataType>string</dataType>
      <allowedValueList>
        <allowedValue>STOPPED</allowedValue>
        <allowedValue>PAUSED_PLAYBACK</allowedValue>
        <allowedValue>PLAYING</allowedValue>
        <allowedValue>TRANSITIONING</allowedValue>
        <allowedValue>NO_MEDIA_PRESENT</allowedValue>
      </allowedValueList>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>CurrentTrackMetaData</name>
      <dataType>string</dataType>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>NextAVTransportURI</name>
      <dataType>string</dataType>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>PossibleRecordQualityModes</name>
      <dataType>string</dataType>
      <allowedValueList>
        <allowedValue>NOT_IMPLEMENTED</allowedValue>
      </allowedValueList>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>CurrentTrack</name>
      <dataType>ui4</dataType>
      <allowedValueRange>
        <minimum>0</minimum>
        <maximum>65535</maximum>
        <step>1</step>
      </allowedValueRange>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>AbsoluteTimePosition</name>
      <dataType>string</dataType>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>NextAVTransportURIMetaData</name>
      <dataType>string</dataType>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>PlaybackStorageMedium</name>
      <dataType>string</dataType>
      <allowedValueList>
        <allowedValue>NONE</allowedValue>
        <allowedValue>UNKNOWN</allowedValue>
        <allowedValue>CD-DA</allowedValue>
        <allowedValue>HDD</allowedValue>
        <allowedValue>NETWORK</allowedValue>
      </allowedValueList>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>CurrentTransportActions</name>
      <dataType>string</dataType>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>RecordMediumWriteStatus</name>
      <dataType>string</dataType>
      <allowedValueList>
        <allowedValue>NOT_IMPLEMENTED</allowedValue>
      </allowedValueList>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>PossiblePlaybackStorageMedia</name>
      <dataType>string</dataType>
      <allowedValueList>
        <allowedValue>NONE</allowedValue>
        <allowedValue>UNKNOWN</allowedValue>
        <allowedValue>CD-DA</allowedValue>
        <allowedValue>HDD</allowedValue>
        <allowedValue>NETWORK</allowedValue>
      </allowedValueList>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>AVTransportURIMetaData</name>
      <dataType>string</dataType>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>NumberOfTracks</name>
      <dataType>ui4</dataType>
      <allowedValueRange>
        <minimum>0</minimum>
        <maximum>65535</maximum>
      </allowedValueRange>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>A_ARG_TYPE_SeekMode</name>
      <dataType>string</dataType>
      <allowedValueList>
        <allowedValue>REL_TIME</allowedValue>
        <allowedValue>TRACK_NR</allowedValue>
      </allowedValueList>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>A_ARG_TYPE_SeekTarget</name>
      <dataType>string</dataType>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>PossibleRecordStorageMedia</name>
      <dataType>string</dataType>
      <allowedValueList>
        <allowedValue>NOT_IMPLEMENTED</allowedValue>
      </allowedValueList>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>TransportStatus</name>
      <dataType>string</dataType>
      <allowedValueList>
        <allowedValue>OK</allowedValue>
        <allowedValue>ERROR_OCCURRED</allowedValue>
      </allowedValueList>
    </stateVariable>
    <stateVariable sendEvents="no">
      <name>TransportPlaySpeed</name>
      <dataType>string</dataType>
      <allowedValueList>
        <allowedValue>1</allowedValue>
      </allowedValueList>
    </stateVariable>
  </serviceStateTable>
</scpd>
```

* actionList 目前服务上所包含的动作列表。
* serviceStateTable 目前服务上所包含的状态变量。

```xml
<action>
    <!-- 动作名称 -->
    <name>Pause</name>
    <!-- 参数列表 -->
    <argumentList>
        <argument>
            <!-- 参数名称 -->
            <name>InstanceID</name>
            <!-- 输出或输出-->
            <direction>in</direction>
            <!-- 声明参数有关的状态变量 -->
            <relatedStateVariable>A_ARG_TYPE_InstanceID</relatedStateVariable>
        </argument>
    </argumentList>
</action>
```

```
<serviceStateTable>
<!-- 状态变量 -->
    <stateVariable>
        <!-- 是否发送事件消息，如果为yes则该状态变量发生变化时生成事件消息。 -->
        <stateVariable sendEvents="no">
        <!-- 状态变量名称 -->
        <name>A_ARG_TYPE_InstanceID</name>
        <!-- 状态数据类型 -->
        <dataType>ui4</dataType>
    </stateVariable>
</serviceStateTable>
```

## 电视屏幕模拟
可以使用 [Kodi](https://kodi.tv/)，Settings - Service Settings - UPnP/DLNA 开启相关功能

## SOAP控制设备

### 设置 URI

```
POST /AVTransport/2e7b079b-8f96-266b-537e-b8f445ee2a6f/control.xml HTTP/1.1
Content-Type: text/xml; charset="utf-8"
SOAPACTION: urn:schemas-upnp-org:service:serviceType:v#SetAVTransportURI
Host: 10.0.206.238:1708
Connection: close
User-Agent: Paw/3.1.5 (Macintosh; OS X/10.13.3) GCDHTTPRequest
Content-Length: 503

<?xml version="1.0" encoding="utf-8" standalone="no"?>
<s:Envelope s:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/" xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
    <s:Body>
        <u:SetAVTransportURI xmlns:u="urn:schemas-upnp-org:service:AVTransport:1">
            <InstanceID>0</InstanceID>
            <CurrentURI>http://localhost:8088/606_hd_enhancements_to_scenekit.mp4</CurrentURI>
            <CurrentURIMetaData />
        </u:SetAVTransportURI>
    </s:Body>
</s:Envelope>
```

### 播放
```
POST /AVTransport/2e7b079b-8f96-266b-537e-b8f445ee2a6f/control.xml HTTP/1.1
Content-Type: text/xml; charset="utf-8"
SOAPACTION: urn:schemas-upnp-org:service:serviceType:v#Play
Host: 10.0.206.238:1708
Connection: close
User-Agent: Paw/3.1.5 (Macintosh; OS X/10.13.3) GCDHTTPRequest
Content-Length: 376

<?xml version="1.0" encoding="utf-8" standalone="no"?>
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/" s:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
    <s:Body>
        <u:Play xmlns:u="urn:schemas-upnp-org:service:AVTransport:1">
            <InstanceID>0</InstanceID>
            <Speed>1</Speed>
        </u:Play>
    </s:Body>
</s:Envelope>
```

### 暂停

```
POST /AVTransport/2e7b079b-8f96-266b-537e-b8f445ee2a6f/control.xml HTTP/1.1
Content-Type: text/xml; charset="utf-8"
SOAPACTION: urn:schemas-upnp-org:service:serviceType:v#Pause
Host: 10.0.206.238:1708
Connection: close
User-Agent: Paw/3.1.5 (Macintosh; OS X/10.13.3) GCDHTTPRequest
Content-Length: 378

<?xml version="1.0" encoding="utf-8" standalone="no"?>
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/" s:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
    <s:Body>
        <u:Pause xmlns:u="urn:schemas-upnp-org:service:AVTransport:1">
            <InstanceID>0</InstanceID>
            <Speed>1</Speed>
        </u:Pause>
    </s:Body>
</s:Envelope>
```

### 跳转
```

POST /AVTransport/2e7b079b-8f96-266b-537e-b8f445ee2a6f/control.xml HTTP/1.1
Content-Type: text/xml; charset="utf-8"
SOAPACTION: urn:schemas-upnp-org:service:serviceType:v#Seek
Host: 10.0.206.238:1708
Connection: close
User-Agent: Paw/3.1.5 (Macintosh; OS X/10.13.3) GCDHTTPRequest
Content-Length: 419

<?xml version="1.0" encoding="utf-8" standalone="no"?>
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/" s:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
    <s:Body>
        <u:Seek xmlns:u="urn:schemas-upnp-org:service:AVTransport:1">
            <InstanceID>0</InstanceID>
            <Unit>REL_TIME</Unit>
            <Target>00:25:21</Target>
        </u:Seek>
    </s:Body>
</s:Envelope>
```

## 参考链接
0. [基于DLNA实现iOS，Android投屏：基本概念](http://ios.jobbole.com/84764/)
0. [基于DLNA实现iOS，Android投屏：SSDP发现设备](http://ios.jobbole.com/84518/)
0. [基于DLNA实现iOS，Android投屏：SOAP控制设备](http://ios.jobbole.com/84519/)
0. [基于DLNA实现iOS，Android投屏：订阅事件通知](http://ios.jobbole.com/84520/)


