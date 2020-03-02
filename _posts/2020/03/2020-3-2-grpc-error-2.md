---
layout: post
title: "grpc报错解决：undefined reference to `google::protobuf::internal::LogMessage::LogMessage(google::protobuf::LogLevel, char const*, int)'"
categories: [行走的问题解决机]
description: ""
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

早上在用 grpc的时候遇到了报错：

```sh
examples.grpc.pb.o: In function `google::protobuf::internal::ToIntSize(unsigned long)':
/usr/local/include/google/protobuf/message_lite.h:106: undefined reference to `google::protobuf::internal::LogMessage::LogMessage(google::protobuf::LogLevel, char const*, int)'
/usr/local/include/google/protobuf/message_lite.h:106: undefined reference to `google::protobuf::internal::LogMessage::operator<<(char const*)'
/usr/local/include/google/protobuf/message_lite.h:106: undefined reference to `google::protobuf::internal::LogFinisher::operator=(google::protobuf::internal::LogMessage&)'
/usr/local/include/google/protobuf/message_lite.h:106: undefined reference to `google::protobuf::internal::LogMessage::~LogMessage()'
/usr/local/include/google/protobuf/message_lite.h:106: undefined reference to `google::protobuf::internal::LogMessage::~LogMessage()'
examples.grpc.pb.o: In function `google::protobuf::io::ZeroCopyOutputStream::ZeroCopyOutputStream()':
/usr/local/include/google/protobuf/io/zero_copy_stream.h:186: undefined reference to `vtable for google::protobuf::io::ZeroCopyOutputStream'
examples.grpc.pb.o: In function `google::protobuf::io::ZeroCopyOutputStream::~ZeroCopyOutputStream()':
/usr/local/include/google/protobuf/io/zero_copy_stream.h:187: undefined reference to `vtable for google::protobuf::io::ZeroCopyOutputStream'
examples.grpc.pb.o: In function `grpc::Status grpc::GenericSerialize<grpc::ProtoBufferWriter, SearchRequest>(google::protobuf::MessageLite const&, grpc::ByteBuffer*, bool*)':
/usr/local/include/grpcpp/impl/codegen/proto_utils.h:56: undefined reference to `google::protobuf::MessageLite::SerializeWithCachedSizesToArray(unsigned char*) const'
/usr/local/include/grpcpp/impl/codegen/proto_utils.h:65: undefined reference to `google::protobuf::MessageLite::SerializeToZeroCopyStream(google::protobuf::io::ZeroCopyOutputStream*) const'
examples.grpc.pb.o:(.data.rel.ro._ZTVN4grpc17ProtoBufferWriterE[_ZTVN4grpc17ProtoBufferWriterE]+0x38): undefined reference to `google::protobuf::io::ZeroCopyOutputStream::WriteAliasedRaw(void const*, int)'
examples.grpc.pb.o:(.data.rel.ro._ZTIN4grpc17ProtoBufferWriterE[_ZTIN4grpc17ProtoBufferWriterE]+0x10): undefined reference to `typeinfo for google::protobuf::io::ZeroCopyOutputStream'
examples.grpc.pb.o: In function `grpc::Status grpc::GenericDeserialize<grpc::ProtoBufferReader, SearchResponse>(grpc::ByteBuffer*, google::protobuf::MessageLite*)':
/usr/local/include/grpcpp/impl/codegen/proto_utils.h:86: undefined reference to `google::protobuf::MessageLite::ParseFromZeroCopyStream(google::protobuf::io::ZeroCopyInputStream*)'
examples.grpc.pb.o: In function `grpc::Status grpc::GenericDeserialize<grpc::ProtoBufferReader, SearchRequest>(grpc::ByteBuffer*, google::protobuf::MessageLite*)':
/usr/local/include/grpcpp/impl/codegen/proto_utils.h:86: undefined reference to `google::protobuf::MessageLite::ParseFromZeroCopyStream(google::protobuf::io::ZeroCopyInputStream*)'
examples.grpc.pb.o: In function `grpc::Status grpc::GenericSerialize<grpc::ProtoBufferWriter, SearchResponse>(google::protobuf::MessageLite const&, grpc::ByteBuffer*, bool*)':
/usr/local/include/grpcpp/impl/codegen/proto_utils.h:56: undefined reference to `google::protobuf::MessageLite::SerializeWithCachedSizesToArray(unsigned char*) const'
/usr/local/include/grpcpp/impl/codegen/proto_utils.h:65: undefined reference to `google::protobuf::MessageLite::SerializeToZeroCopyStream(google::protobuf::io::ZeroCopyOutputStream*) const'
examples.pb.o: In function `InitDefaultsscc_info_SearchRequest_examples_2eproto()':
/home/zhang/grpc-test/examples.pb.cc:26: undefined reference to `google::protobuf::internal::VerifyVersion(int, int, char const*)'
examples.pb.o: In function `InitDefaultsscc_info_SearchResponse_examples_2eproto()':
/home/zhang/grpc-test/examples.pb.cc:40: undefined reference to `google::protobuf::internal::VerifyVersion(int, int, char const*)'
examples.pb.o: In function `SearchRequest::_InternalParse(char const*, google::protobuf::internal::ParseContext*)':
/home/zhang/grpc-test/examples.pb.cc:173: undefined reference to `google::protobuf::internal::InlineGreedyStringParser(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >*, char const*, google::protobuf::internal::ParseContext*)'
/home/zhang/grpc-test/examples.pb.cc:184: undefined reference to `google::protobuf::internal::UnknownFieldParse(unsigned int, google::protobuf::internal::InternalMetadataWithArena*, char const*, google::protobuf::internal::ParseContext*)'
examples.pb.o: In function `SearchRequest::_InternalSerialize(unsigned char*, google::protobuf::io::EpsCopyOutputStream*) const':
/home/zhang/grpc-test/examples.pb.cc:206: undefined reference to `google::protobuf::internal::WireFormatLite::VerifyUtf8String(char const*, int, google::protobuf::internal::WireFormatLite::Operation, char const*)'
/home/zhang/grpc-test/examples.pb.cc:215: undefined reference to `google::protobuf::internal::WireFormat::InternalSerializeUnknownFieldsToArray(google::protobuf::UnknownFieldSet const&, unsigned char*, google::protobuf::io::EpsCopyOutputStream*)'
examples.pb.o: In function `SearchRequest::ByteSizeLong() const':
/home/zhang/grpc-test/examples.pb.cc:239: undefined reference to `google::protobuf::internal::ComputeUnknownFieldsSize(google::protobuf::internal::InternalMetadataWithArena const&, unsigned long, google::protobuf::internal::CachedSize*)'
examples.pb.o: In function `SearchRequest::MergeFrom(google::protobuf::Message const&)':
/home/zhang/grpc-test/examples.pb.cc:248: undefined reference to `google::protobuf::internal::LogMessage::LogMessage(google::protobuf::LogLevel, char const*, int)'
/home/zhang/grpc-test/examples.pb.cc:248: undefined reference to `google::protobuf::internal::LogMessage::operator<<(char const*)'
/home/zhang/grpc-test/examples.pb.cc:248: undefined reference to `google::protobuf::internal::LogFinisher::operator=(google::protobuf::internal::LogMessage&)'
/home/zhang/grpc-test/examples.pb.cc:248: undefined reference to `google::protobuf::internal::LogMessage::~LogMessage()'
/home/zhang/grpc-test/examples.pb.cc:254: undefined reference to `google::protobuf::internal::ReflectionOps::Merge(google::protobuf::Message const&, google::protobuf::Message*)'
/home/zhang/grpc-test/examples.pb.cc:248: undefined reference to `google::protobuf::internal::LogMessage::~LogMessage()'
examples.pb.o: In function `SearchRequest::MergeFrom(SearchRequest const&)':
/home/zhang/grpc-test/examples.pb.cc:263: undefined reference to `google::protobuf::internal::LogMessage::LogMessage(google::protobuf::LogLevel, char const*, int)'
/home/zhang/grpc-test/examples.pb.cc:263: undefined reference to `google::protobuf::internal::LogMessage::operator<<(char const*)'
/home/zhang/grpc-test/examples.pb.cc:263: undefined reference to `google::protobuf::internal::LogFinisher::operator=(google::protobuf::internal::LogMessage&)'
/home/zhang/grpc-test/examples.pb.cc:263: undefined reference to `google::protobuf::internal::LogMessage::~LogMessage()'
/home/zhang/grpc-test/examples.pb.cc:263: undefined reference to `google::protobuf::internal::LogMessage::~LogMessage()'
examples.pb.o: In function `SearchResponse::_InternalParse(char const*, google::protobuf::internal::ParseContext*)':
/home/zhang/grpc-test/examples.pb.cc:372: undefined reference to `google::protobuf::internal::InlineGreedyStringParser(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >*, char const*, google::protobuf::internal::ParseContext*)'
/home/zhang/grpc-test/examples.pb.cc:383: undefined reference to `google::protobuf::internal::UnknownFieldParse(unsigned int, google::protobuf::internal::InternalMetadataWithArena*, char const*, google::protobuf::internal::ParseContext*)'
examples.pb.o: In function `SearchResponse::_InternalSerialize(unsigned char*, google::protobuf::io::EpsCopyOutputStream*) const':
/home/zhang/grpc-test/examples.pb.cc:405: undefined reference to `google::protobuf::internal::WireFormatLite::VerifyUtf8String(char const*, int, google::protobuf::internal::WireFormatLite::Operation, char const*)'
/home/zhang/grpc-test/examples.pb.cc:414: undefined reference to `google::protobuf::internal::WireFormat::InternalSerializeUnknownFieldsToArray(google::protobuf::UnknownFieldSet const&, unsigned char*, google::protobuf::io::EpsCopyOutputStream*)'
examples.pb.o: In function `SearchResponse::ByteSizeLong() const':
/home/zhang/grpc-test/examples.pb.cc:438: undefined reference to `google::protobuf::internal::ComputeUnknownFieldsSize(google::protobuf::internal::InternalMetadataWithArena const&, unsigned long, google::protobuf::internal::CachedSize*)'
examples.pb.o: In function `SearchResponse::MergeFrom(google::protobuf::Message const&)':
/home/zhang/grpc-test/examples.pb.cc:447: undefined reference to `google::protobuf::internal::LogMessage::LogMessage(google::protobuf::LogLevel, char const*, int)'
/home/zhang/grpc-test/examples.pb.cc:447: undefined reference to `google::protobuf::internal::LogMessage::operator<<(char const*)'
/home/zhang/grpc-test/examples.pb.cc:447: undefined reference to `google::protobuf::internal::LogFinisher::operator=(google::protobuf::internal::LogMessage&)'
/home/zhang/grpc-test/examples.pb.cc:447: undefined reference to `google::protobuf::internal::LogMessage::~LogMessage()'
/home/zhang/grpc-test/examples.pb.cc:453: undefined reference to `google::protobuf::internal::ReflectionOps::Merge(google::protobuf::Message const&, google::protobuf::Message*)'
/home/zhang/grpc-test/examples.pb.cc:447: undefined reference to `google::protobuf::internal::LogMessage::~LogMessage()'
examples.pb.o: In function `SearchResponse::MergeFrom(SearchResponse const&)':
/home/zhang/grpc-test/examples.pb.cc:462: undefined reference to `google::protobuf::internal::LogMessage::LogMessage(google::protobuf::LogLevel, char const*, int)'
/home/zhang/grpc-test/examples.pb.cc:462: undefined reference to `google::protobuf::internal::LogMessage::operator<<(char const*)'
/home/zhang/grpc-test/examples.pb.cc:462: undefined reference to `google::protobuf::internal::LogFinisher::operator=(google::protobuf::internal::LogMessage&)'
/home/zhang/grpc-test/examples.pb.cc:462: undefined reference to `google::protobuf::internal::LogMessage::~LogMessage()'
/home/zhang/grpc-test/examples.pb.cc:462: undefined reference to `google::protobuf::internal::LogMessage::~LogMessage()'
examples.pb.o: In function `SearchRequest* google::protobuf::Arena::CreateMaybeMessage<SearchRequest>(google::protobuf::Arena*)':
/usr/local/include/google/protobuf/arena.h:539: undefined reference to `google::protobuf::Arena::AllocateAlignedNoHook(unsigned long)'
/usr/local/include/google/protobuf/arena.h:542: undefined reference to `google::protobuf::internal::ArenaImpl::AllocateAlignedAndAddCleanup(unsigned long, void (*)(void*))'
examples.pb.o: In function `SearchResponse* google::protobuf::Arena::CreateMaybeMessage<SearchResponse>(google::protobuf::Arena*)':
/usr/local/include/google/protobuf/arena.h:539: undefined reference to `google::protobuf::Arena::AllocateAlignedNoHook(unsigned long)'
/usr/local/include/google/protobuf/arena.h:542: undefined reference to `google::protobuf::internal::ArenaImpl::AllocateAlignedAndAddCleanup(unsigned long, void (*)(void*))'
examples.pb.o: In function `__static_initialization_and_destruction_0(int, int)':
/home/zhang/grpc-test/examples.pb.cc:103: undefined reference to `google::protobuf::internal::AddDescriptors(google::protobuf::internal::DescriptorTable const*)'
examples.pb.o: In function `google::protobuf::StringPiece::CheckedSsizeTFromSizeT(unsigned long)':
/usr/local/include/google/protobuf/stubs/stringpiece.h:196: undefined reference to `google::protobuf::StringPiece::LogFatalSizeTooBig(unsigned long, char const*)'
examples.pb.o: In function `google::protobuf::io::EpsCopyOutputStream::WriteStringMaybeAliased(unsigned int, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&, unsigned char*)':
/usr/local/include/google/protobuf/io/coded_stream.h:719: undefined reference to `google::protobuf::io::EpsCopyOutputStream::WriteStringMaybeAliasedOutline(unsigned int, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&, unsigned char*)'
examples.pb.o: In function `google::protobuf::Arena::AllocHook(std::type_info const*, unsigned long) const':
/usr/local/include/google/protobuf/arena.h:526: undefined reference to `google::protobuf::Arena::OnArenaAllocation(std::type_info const*, unsigned long) const'
examples.pb.o: In function `google::protobuf::internal::ArenaStringPtr::CreateInstance(google::protobuf::Arena*, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const*)':
/usr/local/include/google/protobuf/arenastring.h:371: undefined reference to `google::protobuf::internal::LogMessage::LogMessage(google::protobuf::LogLevel, char const*, int)'
/usr/local/include/google/protobuf/arenastring.h:371: undefined reference to `google::protobuf::internal::LogMessage::operator<<(char const*)'
/usr/local/include/google/protobuf/arenastring.h:371: undefined reference to `google::protobuf::internal::LogFinisher::operator=(google::protobuf::internal::LogMessage&)'
/usr/local/include/google/protobuf/arenastring.h:371: undefined reference to `google::protobuf::internal::LogMessage::~LogMessage()'
examples.pb.o: In function `google::protobuf::internal::ArenaStringPtr::CreateInstance(google::protobuf::Arena*, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const*)':
/usr/local/include/google/protobuf/arena.h:539: undefined reference to `google::protobuf::Arena::AllocateAlignedNoHook(unsigned long)'
/usr/local/include/google/protobuf/arena.h:542: undefined reference to `google::protobuf::internal::ArenaImpl::AllocateAlignedAndAddCleanup(unsigned long, void (*)(void*))'
examples.pb.o: In function `google::protobuf::internal::ArenaStringPtr::CreateInstance(google::protobuf::Arena*, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const*)':
/usr/local/include/google/protobuf/arenastring.h:371: undefined reference to `google::protobuf::internal::LogMessage::~LogMessage()'
examples.pb.o: In function `google::protobuf::internal::ArenaStringPtr::CreateInstanceNoArena(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const*)':
/usr/local/include/google/protobuf/arenastring.h:377: undefined reference to `google::protobuf::internal::LogMessage::LogMessage(google::protobuf::LogLevel, char const*, int)'
/usr/local/include/google/protobuf/arenastring.h:377: undefined reference to `google::protobuf::internal::LogMessage::operator<<(char const*)'
/usr/local/include/google/protobuf/arenastring.h:377: undefined reference to `google::protobuf::internal::LogFinisher::operator=(google::protobuf::internal::LogMessage&)'
/usr/local/include/google/protobuf/arenastring.h:377: undefined reference to `google::protobuf::internal::LogMessage::~LogMessage()'
/usr/local/include/google/protobuf/arenastring.h:377: undefined reference to `google::protobuf::internal::LogMessage::~LogMessage()'
examples.pb.o: In function `google::protobuf::internal::GetEmptyStringAlreadyInited[abi:cxx11]()':
/usr/local/include/google/protobuf/message_lite.h:154: undefined reference to `google::protobuf::internal::fixed_address_empty_string[abi:cxx11]'
examples.pb.o: In function `google::protobuf::MessageLite::MessageLite()':
/usr/local/include/google/protobuf/message_lite.h:186: undefined reference to `vtable for google::protobuf::MessageLite'
examples.pb.o: In function `google::protobuf::MessageLite::~MessageLite()':
/usr/local/include/google/protobuf/message_lite.h:187: undefined reference to `vtable for google::protobuf::MessageLite'
examples.pb.o: In function `google::protobuf::internal::InitSCC(google::protobuf::internal::SCCInfoBase*)':
/usr/local/include/google/protobuf/generated_message_util.h:237: undefined reference to `google::protobuf::internal::InitSCCImpl(google::protobuf::internal::SCCInfoBase*)'
examples.pb.o: In function `google::protobuf::internal::OnShutdownDestroyMessage(void const*)':
/usr/local/include/google/protobuf/generated_message_util.h:244: undefined reference to `google::protobuf::internal::DestroyMessage(void const*)'
/usr/local/include/google/protobuf/generated_message_util.h:244: undefined reference to `google::protobuf::internal::OnShutdownRun(void (*)(void const*), void const*)'
examples.pb.o: In function `google::protobuf::internal::EpsCopyInputStream::DoneWithCheck(char const**, int)':
/usr/local/include/google/protobuf/parse_context.h:209: undefined reference to `google::protobuf::internal::LogMessage::LogMessage(google::protobuf::LogLevel, char const*, int)'
/usr/local/include/google/protobuf/parse_context.h:209: undefined reference to `google::protobuf::internal::LogMessage::operator<<(char const*)'
/usr/local/include/google/protobuf/parse_context.h:209: undefined reference to `google::protobuf::internal::LogFinisher::operator=(google::protobuf::internal::LogMessage&)'
/usr/local/include/google/protobuf/parse_context.h:209: undefined reference to `google::protobuf::internal::LogMessage::~LogMessage()'
/usr/local/include/google/protobuf/parse_context.h:213: undefined reference to `google::protobuf::internal::EpsCopyInputStream::DoneFallback(char const*, int)'
/usr/local/include/google/protobuf/parse_context.h:209: undefined reference to `google::protobuf::internal::LogMessage::~LogMessage()'
examples.pb.o: In function `google::protobuf::internal::ReadTag(char const*, unsigned int*, unsigned int)':
/usr/local/include/google/protobuf/parse_context.h:510: undefined reference to `google::protobuf::internal::ReadTagFallback(char const*, unsigned int)'
examples.pb.o: In function `google::protobuf::internal::VerifyUTF8(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const*, char const*)':
/usr/local/include/google/protobuf/parse_context.h:635: undefined reference to `google::protobuf::internal::VerifyUTF8(google::protobuf::StringPiece, char const*)'
examples.pb.o: In function `google::protobuf::UnknownFieldSet::Clear()':
/usr/local/include/google/protobuf/unknown_field_set.h:328: undefined reference to `google::protobuf::UnknownFieldSet::ClearFallback()'
examples.pb.o: In function `google::protobuf::internal::InternalMetadataWithArena::DoMergeFrom(google::protobuf::UnknownFieldSet const&)':
/usr/local/include/google/protobuf/metadata.h:64: undefined reference to `google::protobuf::UnknownFieldSet::MergeFrom(google::protobuf::UnknownFieldSet const&)'
examples.pb.o: In function `google::protobuf::internal::InternalMetadataWithArena::default_instance()':
/usr/local/include/google/protobuf/metadata.h:70: undefined reference to `google::protobuf::UnknownFieldSet::default_instance()'
examples.pb.o: In function `google::protobuf::Message::Message()':
/usr/local/include/google/protobuf/message.h:207: undefined reference to `vtable for google::protobuf::Message'
examples.pb.o: In function `google::protobuf::Message::~Message()':
/usr/local/include/google/protobuf/message.h:208: undefined reference to `vtable for google::protobuf::Message'
examples.pb.o: In function `SearchRequest::GetMetadataStatic()':
/home/zhang/grpc-test/examples.pb.h:165: undefined reference to `google::protobuf::internal::AssignDescriptors(google::protobuf::internal::DescriptorTable const*)'
examples.pb.o: In function `SearchResponse::GetMetadataStatic()':
/home/zhang/grpc-test/examples.pb.h:300: undefined reference to `google::protobuf::internal::AssignDescriptors(google::protobuf::internal::DescriptorTable const*)'
examples.pb.o: In function `SearchRequest const* google::protobuf::DynamicCastToGenerated<SearchRequest>(google::protobuf::Message const*)':
/usr/local/include/google/protobuf/message.h:1200: undefined reference to `typeinfo for google::protobuf::Message'
examples.pb.o: In function `SearchResponse const* google::protobuf::DynamicCastToGenerated<SearchResponse>(google::protobuf::Message const*)':
/usr/local/include/google/protobuf/message.h:1200: undefined reference to `typeinfo for google::protobuf::Message'
examples.pb.o: In function `google::protobuf::internal::InternalMetadataWithArenaBase<google::protobuf::UnknownFieldSet, google::protobuf::internal::InternalMetadataWithArena>::mutable_unknown_fields_slow()':
/usr/local/include/google/protobuf/arena.h:539: undefined reference to `google::protobuf::Arena::AllocateAlignedNoHook(unsigned long)'
/usr/local/include/google/protobuf/arena.h:542: undefined reference to `google::protobuf::internal::ArenaImpl::AllocateAlignedAndAddCleanup(unsigned long, void (*)(void*))'
examples.pb.o:(.data.rel.ro._ZTV14SearchResponse[_ZTV14SearchResponse]+0x20): undefined reference to `google::protobuf::Message::GetTypeName[abi:cxx11]() const'
examples.pb.o:(.data.rel.ro._ZTV14SearchResponse[_ZTV14SearchResponse]+0x58): undefined reference to `google::protobuf::Message::InitializationErrorString[abi:cxx11]() const'
examples.pb.o:(.data.rel.ro._ZTV14SearchResponse[_ZTV14SearchResponse]+0x60): undefined reference to `google::protobuf::Message::CheckTypeAndMergeFrom(google::protobuf::MessageLite const&)'
examples.pb.o:(.data.rel.ro._ZTV14SearchResponse[_ZTV14SearchResponse]+0xa0): undefined reference to `google::protobuf::Message::DiscardUnknownFields()'
examples.pb.o:(.data.rel.ro._ZTV14SearchResponse[_ZTV14SearchResponse]+0xa8): undefined reference to `google::protobuf::Message::SpaceUsedLong() const'
examples.pb.o:(.data.rel.ro._ZTV13SearchRequest[_ZTV13SearchRequest]+0x20): undefined reference to `google::protobuf::Message::GetTypeName[abi:cxx11]() const'
examples.pb.o:(.data.rel.ro._ZTV13SearchRequest[_ZTV13SearchRequest]+0x58): undefined reference to `google::protobuf::Message::InitializationErrorString[abi:cxx11]() const'
examples.pb.o:(.data.rel.ro._ZTV13SearchRequest[_ZTV13SearchRequest]+0x60): undefined reference to `google::protobuf::Message::CheckTypeAndMergeFrom(google::protobuf::MessageLite const&)'
examples.pb.o:(.data.rel.ro._ZTV13SearchRequest[_ZTV13SearchRequest]+0xa0): undefined reference to `google::protobuf::Message::DiscardUnknownFields()'
examples.pb.o:(.data.rel.ro._ZTV13SearchRequest[_ZTV13SearchRequest]+0xa8): undefined reference to `google::protobuf::Message::SpaceUsedLong() const'
examples.pb.o:(.data.rel.ro._ZTI14SearchResponse[_ZTI14SearchResponse]+0x10): undefined reference to `typeinfo for google::protobuf::Message'
examples.pb.o:(.data.rel.ro._ZTI13SearchRequest[_ZTI13SearchRequest]+0x10): undefined reference to `typeinfo for google::protobuf::Message'
collect2: error: ld returned 1 exit status
Makefile:44: recipe for target 'client' failed
make: *** [client] Error 1
```


解决不了，直接重装系统，重新编译。






{% raw %}
***          
{% endraw %}

关于`grpc`与`protobuf`的安装和使用，参考这篇博客：[grpc与protobuf最佳实践](http://zhang0peter.com/grpc-and-protobuf/)