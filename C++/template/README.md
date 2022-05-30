```cpp
#if 0
class CacInfoField { };

class CacMsgType : public CacInfoField
{
public:
	CacMsgType(CacInfoMsg_MsgType value) { this->value = value; }

	CacInfoMsg_MsgType value;
};

class CacVnId : public CacInfoField
{
public:
	CacVnId(unsigned short value) { this->value = value; }

	unsigned short value;
};

class CacTgId : public CacInfoField
{
public:
	CacTgId(unsigned short value) { this->value = value; }

	unsigned short value;
};

class CacCallId : public CacInfoField
{
public:
	CacCallId(std::string &value) { this->value = value; };
	CacCallId(const char *value) { this->value = (value == nullptr)? "": std::string(value); };
	CacCallId(const char *value, size_t len) { this->value = (value == nullptr || len == 0)? "": std::string(value, len); };

	std::string value;
};

class CacBandwidth : public CacInfoField
{
public:
	CacBandwidth(uint32_t value) { this->value = value; };

	uint32_t value;
};

class CacAudioBandwidth : public CacInfoField
{
public:
	CacAudioBandwidth(uint32_t value) { this->value = value; };

	uint32_t value;
};

class CacVideoBandwidth : public CacInfoField
{
public:
	CacVideoBandwidth(uint32_t value) { this->value = value; };

	uint32_t value;
};

class CacProhibitReject : public CacInfoField
{
public:
	CacProhibitReject(bool value) { this->value = value; };

	bool value;
};

class CacReject : public CacInfoField
{
public:
	CacReject(CacInfoMsg_RejectType value) { this->value = value; };
	CacInfoMsg_RejectType value;
};

class CacNewCall : public CacInfoField
{
public:
	CacNewCall(bool value) { this->value = value; };
	bool value;
};

class CacLastBandwidth : public CacInfoField
{
public:
	CacLastBandwidth(uint32_t value) { this->value = value; };

	uint32_t value;
};

class CacLastAudioBandwidth : public CacInfoField
{
public:
	CacLastAudioBandwidth(uint32_t value) { this->value = value; };

	uint32_t value;
};

class CacLastVideoBandwidth : public CacInfoField
{
public:
	CacLastVideoBandwidth(uint32_t value) { this->value = value; };

	uint32_t value;
};

class CacInCallCnt : public CacInfoField
{
public:
	CacInCallCnt(uint32_t value) { this->value = value; };

	uint32_t value;
};

class CacOutCallCnt : public CacInfoField
{
public:
	CacOutCallCnt(uint32_t value) { this->value = value; };

	uint32_t value;
};

class CacTotalCallCnt : public CacInfoField
{
public:
	CacTotalCallCnt(uint32_t value) { this->value = value; };

	uint32_t value;
};

class CacInBwCnt : public CacInfoField
{
public:
	CacInBwCnt(uint32_t value) { this->value = value; };

	uint32_t value;
};

class CacOutBwCnt : public CacInfoField
{
public:
	CacOutBwCnt(uint32_t value) { this->value = value; };

	uint32_t value;
};

class CacTotalBwCnt : public CacInfoField
{
public:
	CacTotalBwCnt(uint32_t value) { this->value = value; };

	uint32_t value;
};

class CacAudioBwCnt : public CacInfoField
{
public:
	CacAudioBwCnt(uint32_t value) { this->value = value; };

	uint32_t value;
};

class CacVideoBwCnt : public CacInfoField
{
public:
	CacVideoBwCnt(uint32_t value) { this->value = value; };

	uint32_t value;
};

class CacInCpsCnt : public CacInfoField
{
public:
	CacInCpsCnt(uint32_t value) { this->value = value; };

	uint32_t value;
};

class CacOutCpsCnt : public CacInfoField
{
public:
	CacOutCpsCnt(uint32_t value) { this->value = value; };

	uint32_t value;
};

class CacTotalCpsCnt : public CacInfoField
{
public:
	CacTotalCpsCnt(uint32_t value) { this->value = value; };

	uint32_t value;
};

inline void cacInfoFieldHandler(CacMsg &cacMsg) {}
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacMsgType value) { cacMsg.set_msgtype(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacVnId value) { cacMsg.set_vnid(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacTgId value) { cacMsg.set_tgid(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacCallId value) { cacMsg.set_callid(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacBandwidth value) { cacMsg.set_bandwidth(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacAudioBandwidth value) { cacMsg.set_audiobandwidth(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacVideoBandwidth value) { cacMsg.set_videobandwidth(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacProhibitReject value) { cacMsg.set_prohibitreject(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacReject value) { cacMsg.set_rejected(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacNewCall value) { cacMsg.set_newcall(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacLastBandwidth value) { cacMsg.set_lastbandwidth(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacLastAudioBandwidth value) { cacMsg.set_lastaudiobandwidth(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacLastVideoBandwidth value) { cacMsg.set_lastvideobandwidth(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacInCallCnt value) { cacMsg.set_in_call_cnt(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacOutCallCnt value) { cacMsg.set_out_call_cnt(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacTotalCallCnt value) { cacMsg.set_total_call_cnt(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacInBwCnt value) { cacMsg.set_in_bw_cnt(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacOutBwCnt value) { cacMsg.set_out_bw_cnt(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacTotalBwCnt value) { cacMsg.set_total_bw_cnt(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacAudioBwCnt value) { cacMsg.set_audio_bw_cnt(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacVideoBwCnt value) { cacMsg.set_video_bw_cnt(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacInCpsCnt value) { cacMsg.set_in_cps_cnt(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacOutCpsCnt value) { cacMsg.set_out_cps_cnt(value.value); }
inline void cacInfoFieldHandler(CacMsg &cacMsg, CacTotalCpsCnt value) { cacMsg.set_total_cps_cnt(value.value); }

inline void cacInfoFieldEncode(CacMsg &cacMsg) { cacInfoFieldHandler(cacMsg); }

template<typename T, typename ... Args>
inline void cacInfoFieldEncode(CacMsg &cacMsg, T&& arg, Args&& ... args)
{
	cacInfoFieldHandler(cacMsg, std::forward<T>(arg));
	cacInfoFieldEncode(cacMsg, std::forward<Args>(args) ...);
}
inline int testCACproto_utils()
{
    CacMsg cacMsg;

    /* 
     * cacInfoFieldEncode(cacMsg, CacMsgType(CacInfoMsg_MsgType_ADD), CacVnId(1), CacTgId(2));
     * cacInfoFieldEncode(cacMsg, CacCallId("350@10.66.18.55-1258"), CacProhibitReject(false), CacReject(false));
     * cacInfoFieldEncode(cacMsg, CacBandwidth(600), CacAudioBandwidth(200), CacVideoBandwidth(300));
     */
    cacInfoFieldEncode(cacMsg,
                       CacMsgType(CacInfoMsg_MsgType_Request),
                       CacVnId(1),
                       CacTgId(2),
                       CacCallId("350@10.66.18.55-1258"),
                       CacProhibitReject(false),
                       CacReject(CacInfoMsg_RejectType_IncomingSessionCps),
                       CacBandwidth(600),
                       CacAudioBandwidth(200),
                       CacVideoBandwidth(300),
                       CacNewCall(true));

    std::cout << "CacMsg built out:" << std::endl;
    std::cout << cacMsg.DebugString() << std::endl;

    char * buffer = cacQueryDataMsgBuild(cacMsg);
    if (buffer == nullptr)
    {
        std::cout << "Allocate CACBuffer Fail" << std::endl;
        return -1;
    }

    CacMsg cacInfoMsgOut;
    
    cacQueryDataMsgDecode(cacInfoMsgOut, buffer, cacMsg.ByteSizeLong());

    std::cout << cacInfoMsgOut.DebugString() << std::endl;
    
    std::cout << "msgType :" << cacInfoMsgOut.msgtype() << std::endl;
    std::cout << "vn_id :"    << cacInfoMsgOut.vnid() << std::endl;
    std::cout << "tg_id :" << cacInfoMsgOut.tgid() << std::endl;
    std::cout << "callid :" << cacInfoMsgOut.callid() << std::endl;
    std::cout << "bandwidth :" << cacInfoMsgOut.bandwidth() << std::endl;
    std::cout << "audio_bandwidth :" << cacInfoMsgOut.audiobandwidth() << std::endl;
    std::cout << "video_bandwidth :" << cacInfoMsgOut.videobandwidth() << std::endl;
    std::cout << "prohibit_reject :" << cacInfoMsgOut.prohibitreject() << std::endl;
    std::cout << "rejected :" << cacInfoMsgOut.rejected() << std::endl;
    std::cout << "rejected :" << cacInfoMsgOut.newcall() << std::endl;

    cacQueryDataMsgFree(buffer);
    std::cout << "test ========================" << std::endl;
    return 0;
}
#endif
```
