---
layout: post
comments: true
categories: zimbra
---


## Cấu hình thiết lập relay cho Zimbra

1.  Kiểm tra thiết lập zimbraMtaTransportMaps hiện tại trước khi cấu hình

      $ zmprov gs `zmhostname` zimbraMtaTransportMaps
      zimbraMtaTransportMaps: proxy:ldap:/opt/zimbra/conf/ldap-transport.cf

2.  Cấu hình lại zimbraMtaTransportMaps để tra cứu bảng lookup trước khi gửi

      $ zmprov ms `zmhostname` zimbraMtaTransportMaps 'lmdb:/opt/zimbra/common/conf/rlm,proxy:ldap:/opt/zimbra/conf/ldap-transport.cf'

3. Cấu hình zimbraMtaSmtpSaslPasswordMaps để sử dụng tài khoản SMTP xác thực khi relay

      $ zmprov ms `zmhostname` zimbraMtaSmtpSaslPasswordMaps 'lmdb:/opt/zimbra/common/conf/sasl'

4.  Cấu hình yêu cầu sử dụng xác thực SASL

      $ zmprov ms `zmhostname` zimbraMtaSmtpSaslAuthEnable yes

5.  Cho phép xác thực bằng các lệnh AUTH/LOGIN

      $ zmprov ms `zmhostname` zimbraMtaSmtpSaslSecurityOptions noanonymous

## Kiểm tra lại các thiết lập trên

      $ zmprov gs `zmhostname` zimbraMtaTransportMaps
      zimbraMtaTransportMaps: lmdb:/opt/zimbra/common/conf/rlm,proxy:ldap:/opt/zimbra/conf/ldap-transport.cf

      $ zmprov gs `zmhostname` zimbraMtaSmtpSaslPasswordMaps
      zimbraMtaSmtpSaslPasswordMaps: lmdb:/opt/zimbra/common/conf/sasl

      $ zmprov gs `zmhostname` zimbraMtaSmtpSaslAuthEnable
      zimbraMtaSmtpSaslAuthEnable: yes

      $ zmprov gs `zmhostname` zimbraMtaSmtpSaslSecurityOptions
      zimbraMtaSmtpSaslSecurityOptions: noanonymous
