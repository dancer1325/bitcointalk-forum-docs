# Is there a way to automate bitcoin payments for a website?

**Topic ID:** 112

**Total Posts:** 8

---

## Post #1

**Author:** Minsc

**Date:** April 15, 2010, 12:12:40 AM

Is there a way to automate bitcoin payments for a website?
April 15, 2010, 12:12:40 AM
#1
I run a website hosted on freebsd and well right now nobody visits it ever and so I can totally change it.  I am wanting to change it to allow people to pay for services from the website exclusively through bitcoins.  Now I see bitcoin has some linux program but I'm not sure that's compatible with FreeBSD or not.  I also don't know how I can integrate bitcoin into a website to automatically take the bitcoin payments so they show up immediately.  I'd really like them to work so my website immediately recognizes the payment coming in.

---

## Post #2

**Author:** dwdollar

**Date:** April 15, 2010, 12:16:22 AM

Re: Is there a way to automate bitcoin payments for a website?
April 15, 2010, 12:16:22 AM
#2
Not sure about FreeBSD, but payments can be automated with the new version.
http://bitcointalk.org/index.php?topic=63.0

---

## Post #3

**Author:** The Madhatter

**Date:** April 15, 2010, 12:49:20 AM

Re: Is there a way to automate bitcoin payments for a website?
April 15, 2010, 12:49:20 AM
#3
I have the latest SVN snapshot running on FreeBSD (bitcoind) with automation running on a few servers.
You just install databases/db48, x11-toolkits/gtk20, devel/gmake, and devel/boost-all from the ports tree, download wxwidgets 2.9 and install it manually (the port version is 2.8.. that's no good), change a few paths in the makefile.unix file, and run "gmake -f makefile.unix bitcoind". You will end up with a binary called "bitcoind" (it is headless). Run "strip ./bitcoind" to make it smaller (it is really big with the debugging symbols). You can, of course, leave the debugging symbols in. I had to use gdb on bitcoind a few times in the past to figure out a strange crash.
I have tested bitcoind on 32 and 64 bit systems running FreeBSD. I didn't detect any memory leaks or see any crashes.

---

## Post #4

**Author:** generica

**Date:** April 21, 2010, 04:53:40 PM

Re: Is there a way to automate bitcoin payments for a website?
April 21, 2010, 04:53:40 PM
#4
madhatter, thanks for these instructions.  I edited the makefile and installed the dependencies.  Here are the changes I had to make if anyone is interested:
- Added these three INCLUDEPATH entries:
-I"/usr/local/lib/wx/include/gtk2-unicode-release-2.9" \
-I"/usr/local/include/db48" \
-I"/usr/local/include"
- Added this LIBPATH entry:
-L"/usr/local/lib/db48"
- Changed "wx_gtk2ud-2.9" to "wx_gtk2u-2.9" because I didn't build wxwidgets with debugging
This is with SVN Rev 75.  However, the end result won't compile:
Code:
[root@colo /usr/src/bitcoin/trunk]# gmake -f makefile.unix bitcoind
g++ -c -O0 -Wno-invalid-offsetof -Wformat -g -D__WXDEBUG__ -D__WXGTK__ -DNOPCH -I"/usr/include" -I"/usr/local/include/wx-2.9" -I"/usr/local/lib/wx/include/gtk2-unicode-release-2.9" -I"/usr/local/include/db48" -I"/usr/local/include"  -DwxUSE_GUI=0 -o obj/nogui/util.o util.cpp
g++ -c -O0 -Wno-invalid-offsetof -Wformat -g -D__WXDEBUG__ -D__WXGTK__ -DNOPCH -I"/usr/include" -I"/usr/local/include/wx-2.9" -I"/usr/local/lib/wx/include/gtk2-unicode-release-2.9" -I"/usr/local/include/db48" -I"/usr/local/include"  -DwxUSE_GUI=0 -o obj/nogui/script.o script.cpp
g++ -c -O0 -Wno-invalid-offsetof -Wformat -g -D__WXDEBUG__ -D__WXGTK__ -DNOPCH -I"/usr/include" -I"/usr/local/include/wx-2.9" -I"/usr/local/lib/wx/include/gtk2-unicode-release-2.9" -I"/usr/local/include/db48" -I"/usr/local/include"  -DwxUSE_GUI=0 -o obj/nogui/db.o db.cpp
g++ -c -O0 -Wno-invalid-offsetof -Wformat -g -D__WXDEBUG__ -D__WXGTK__ -DNOPCH -I"/usr/include" -I"/usr/local/include/wx-2.9" -I"/usr/local/lib/wx/include/gtk2-unicode-release-2.9" -I"/usr/local/include/db48" -I"/usr/local/include"  -DwxUSE_GUI=0 -o obj/nogui/net.o net.cpp
g++ -c -O0 -Wno-invalid-offsetof -Wformat -g -D__WXDEBUG__ -D__WXGTK__ -DNOPCH -I"/usr/include" -I"/usr/local/include/wx-2.9" -I"/usr/local/lib/wx/include/gtk2-unicode-release-2.9" -I"/usr/local/include/db48" -I"/usr/local/include"  -DwxUSE_GUI=0 -o obj/nogui/irc.o irc.cpp
g++ -c -O0 -Wno-invalid-offsetof -Wformat -g -D__WXDEBUG__ -D__WXGTK__ -DNOPCH -I"/usr/include" -I"/usr/local/include/wx-2.9" -I"/usr/local/lib/wx/include/gtk2-unicode-release-2.9" -I"/usr/local/include/db48" -I"/usr/local/include"  -DwxUSE_GUI=0 -o obj/nogui/main.o main.cpp
g++ -c -O0 -Wno-invalid-offsetof -Wformat -g -D__WXDEBUG__ -D__WXGTK__ -DNOPCH -I"/usr/include" -I"/usr/local/include/wx-2.9" -I"/usr/local/lib/wx/include/gtk2-unicode-release-2.9" -I"/usr/local/include/db48" -I"/usr/local/include"  -DwxUSE_GUI=0 -o obj/nogui/rpc.o rpc.cpp
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/less.hpp: In instantiation of 'boost::mpl::less_impl<mpl_::integral_c_tag, mpl_::integral_c_tag>::apply<mpl_::size_t<8ul>, mpl_::size_t<1ul> >':
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/less.hpp:73:   instantiated from 'boost::mpl::less<mpl_::size_t<8ul>, mpl_::size_t<1ul> >'
/usr/local/include/boost/mpl/aux_/has_type.hpp:20:   instantiated from 'boost::mpl::aux::has_type<boost::mpl::less<mpl_::size_t<8ul>, mpl_::size_t<1ul> >, mpl_::bool_<true> >'
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/quote.hpp:56:   instantiated from 'boost::mpl::quote2<boost::mpl::less, mpl_::void_>::apply<mpl_::size_t<8ul>, mpl_::size_t<1ul> >'
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/apply_wrap.hpp:49:   instantiated from 'boost::mpl::apply_wrap2<boost::mpl::quote2<boost::mpl::less, mpl_::void_>, mpl_::size_t<8ul>, mpl_::size_t<1ul> >'
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/bind.hpp:207:   instantiated from 'boost::mpl::bind2<boost::mpl::quote2<boost::mpl::less, mpl_::void_>, mpl_::arg<-0x00000000000000001>, mpl_::arg<-0x00000000000000001> >::apply<mpl_::size_t<8ul>, mpl_::size_t<1ul>, mpl_::na, mpl_::na, mpl_::na>'
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/apply_wrap.hpp:49:   instantiated from 'boost::mpl::apply_wrap2<boost::mpl::protect<boost::mpl::bind2<boost::mpl::quote2<boost::mpl::less, mpl_::void_>, mpl_::arg<-0x00000000000000001>, mpl_::arg<-0x00000000000000001> >, 0>, mpl_::size_t<8ul>, mpl_::size_t<1ul> >'
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/apply.hpp:73:   instantiated from 'boost::mpl::apply2<boost::mpl::less<mpl_::arg<-0x00000000000000001>, mpl_::arg<-0x00000000000000001> >, mpl_::size_t<8ul>, mpl_::size_t<1ul> >'
/usr/local/include/boost/mpl/max_element.hpp:42:   instantiated from 'boost::mpl::aux::select_max<boost::mpl::less<mpl_::arg<-0x00000000000000001>, mpl_::arg<-0x00000000000000001> > >::apply<boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<6l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<5l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<4l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<3l>, mpl_::size_t<1ul>, boost::mpl::l_item<mpl_::long_<2l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<1l>, mpl_::size_t<8ul>, boost::mpl::l_end> > > > > > >, boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<3l>, mpl_::size_t<1ul>, boost::mpl::l_item<mpl_::long_<2l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<1l>, mpl_::size_t<8ul>, boost::mpl::l_end> > > > >'
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/apply_wrap.hpp:49:   instantiated from 'boost::mpl::apply_wrap2<boost::mpl::protect<boost::mpl::aux::select_max<boost::mpl::less<mpl_::arg<-0x00000000000000001>, mpl_::arg<-0x00000000000000001> > >, 0>, boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<6l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<5l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<4l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<3l>, mpl_::size_t<1ul>, boost::mpl::l_item<mpl_::long_<2l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<1l>, mpl_::size_t<8ul>, boost::mpl::l_end> > > > > > >, boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<3l>, mpl_::size_t<1ul>, boost::mpl::l_item<mpl_::long_<2l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<1l>, mpl_::size_t<8ul>, boost::mpl::l_end> > > > >'
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/apply.hpp:73:   instantiated from 'boost::mpl::apply2<boost::mpl::protect<boost::mpl::aux::select_max<boost::mpl::less<mpl_::arg<-0x00000000000000001>, mpl_::arg<-0x00000000000000001> > >, 0>, boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<6l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<5l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<4l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<3l>, mpl_::size_t<1ul>, boost::mpl::l_item<mpl_::long_<2l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<1l>, mpl_::size_t<8ul>, boost::mpl::l_end> > > > > > >, boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<3l>, mpl_::size_t<1ul>, boost::mpl::l_item<mpl_::long_<2l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<1l>, mpl_::size_t<8ul>, boost::mpl::l_end> > > > >'
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/iter_fold_impl.hpp:115:   instantiated from 'boost::mpl::aux::iter_fold_impl<4, boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<6l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<5l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<4l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<3l>, mpl_::size_t<1ul>, boost::mpl::l_item<mpl_::long_<2l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<1l>, mpl_::size_t<8ul>, boost::mpl::l_end> > > > > > >, boost::mpl::l_iter<boost::mpl::l_end>, boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<6l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<5l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<4l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<3l>, mpl_::size_t<1ul>, boost::mpl::l_item<mpl_::long_<2l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<1l>, mpl_::size_t<8ul>, boost::mpl::l_end> > > > > > >, boost::mpl::protect<boost::mpl::aux::select_max<boost::mpl::less<mpl_::arg<-0x00000000000000001>, mpl_::arg<-0x00000000000000001> > >, 0> >'
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/iter_fold_impl.hpp:146:   instantiated from 'boost::mpl::aux::iter_fold_impl<6, boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<6l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<5l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<4l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<3l>, mpl_::size_t<1ul>, boost::mpl::l_item<mpl_::long_<2l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<1l>, mpl_::size_t<8ul>, boost::mpl::l_end> > > > > > >, boost::mpl::l_iter<boost::mpl::l_end>, boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<6l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<5l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<4l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<3l>, mpl_::size_t<1ul>, boost::mpl::l_item<mpl_::long_<2l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<1l>, mpl_::size_t<8ul>, boost::mpl::l_end> > > > > > >, boost::mpl::protect<boost::mpl::aux::select_max<boost::mpl::less<mpl_::arg<-0x00000000000000001>, mpl_::arg<-0x00000000000000001> > >, 0> >'
/usr/local/include/boost/mpl/iter_fold.hpp:40:   instantiated from 'boost::mpl::iter_fold<boost::mpl::l_item<mpl_::long_<6l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<5l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<4l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<3l>, mpl_::size_t<1ul>, boost::mpl::l_item<mpl_::long_<2l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<1l>, mpl_::size_t<8ul>, boost::mpl::l_end> > > > > >, boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<6l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<5l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<4l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<3l>, mpl_::size_t<1ul>, boost::mpl::l_item<mpl_::long_<2l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<1l>, mpl_::size_t<8ul>, boost::mpl::l_end> > > > > > >, boost::mpl::protect<boost::mpl::aux::select_max<boost::mpl::less<mpl_::arg<-0x00000000000000001>, mpl_::arg<-0x00000000000000001> > >, 0> >'
/usr/local/include/boost/mpl/max_element.hpp:65:   instantiated from 'boost::mpl::max_element<boost::mpl::l_item<mpl_::long_<6l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<5l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<4l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<3l>, mpl_::size_t<1ul>, boost::mpl::l_item<mpl_::long_<2l>, mpl_::size_t<8ul>, boost::mpl::l_item<mpl_::long_<1l>, mpl_::size_t<8ul>, boost::mpl::l_end> > > > > >, boost::mpl::less<mpl_::arg<-0x00000000000000001>, mpl_::arg<-0x00000000000000001> > >'
/usr/local/include/boost/variant/variant.hpp:123:   instantiated from 'boost::detail::variant::max_value<boost::mpl::l_item<mpl_::long_<6l>, std::basic_string<char, std::char_traits<char>, std::allocator<char> >, boost::mpl::l_item<mpl_::long_<5l>, boost::recursive_wrapper<std::vector<json_spirit::Pair_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > >, std::allocator<json_spirit::Pair_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > > > > >, boost::mpl::l_item<mpl_::long_<4l>, boost::recursive_wrapper<std::vector<json_spirit::Value_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > >, std::allocator<json_spirit::Value_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > > > > >, boost::mpl::l_item<mpl_::long_<3l>, bool, boost::mpl::l_item<mpl_::long_<2l>, long int, boost::mpl::l_item<mpl_::long_<1l>, double, boost::mpl::l_end> > > > > >, boost::mpl::sizeof_<mpl_::arg<1> > >'
/usr/local/include/boost/variant/variant.hpp:232:   instantiated from 'boost::detail::variant::make_storage<boost::mpl::l_item<mpl_::long_<6l>, std::basic_string<char, std::char_traits<char>, std::allocator<char> >, boost::mpl::l_item<mpl_::long_<5l>, boost::recursive_wrapper<std::vector<json_spirit::Pair_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > >, std::allocator<json_spirit::Pair_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > > > > >, boost::mpl::l_item<mpl_::long_<4l>, boost::recursive_wrapper<std::vector<json_spirit::Value_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > >, std::allocator<json_spirit::Value_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > > > > >, boost::mpl::l_item<mpl_::long_<3l>, bool, boost::mpl::l_item<mpl_::long_<2l>, long int, boost::mpl::l_item<mpl_::long_<1l>, double, boost::mpl::l_end> > > > > >, boost::variant<std::basic_string<char, std::char_traits<char>, std::allocator<char> >, boost::recursive_wrapper<std::vector<json_spirit::Pair_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > >, std::allocator<json_spirit::Pair_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > > > > >, boost::recursive_wrapper<std::vector<json_spirit::Value_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > >, std::allocator<json_spirit::Value_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > > > > >, bool, long int, double, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_>::has_fallback_type_>'
/usr/local/include/boost/variant/variant.hpp:1098:   instantiated from 'boost::variant<std::basic_string<char, std::char_traits<char>, std::allocator<char> >, boost::recursive_wrapper<std::vector<json_spirit::Pair_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > >, std::allocator<json_spirit::Pair_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > > > > >, boost::recursive_wrapper<std::vector<json_spirit::Value_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > >, std::allocator<json_spirit::Value_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > > > > >, bool, long int, double, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_>'
json/json_spirit_value.h:89:   instantiated from 'json_spirit::Value_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > >'
rpc.cpp:34:   instantiated from here
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/less.hpp:90: warning: comparison between 'enum mpl_::size_t<1ul>::<anonymous>' and 'enum mpl_::size_t<8ul>::<anonymous>'
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/less.hpp: In instantiation of 'boost::mpl::less_impl<mpl_::integral_c_tag, mpl_::integral_c_tag>::apply<boost::integral_constant<long unsigned int, 8ul>, boost::integral_constant<long unsigned int, 1ul> >':
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/less.hpp:73:   instantiated from 'boost::mpl::less<boost::integral_constant<long unsigned int, 8ul>, boost::integral_constant<long unsigned int, 1ul> >'
/usr/local/include/boost/mpl/aux_/has_type.hpp:20:   instantiated from 'boost::mpl::aux::has_type<boost::mpl::less<boost::integral_constant<long unsigned int, 8ul>, boost::integral_constant<long unsigned int, 1ul> >, mpl_::bool_<true> >'
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/quote.hpp:56:   instantiated from 'boost::mpl::quote2<boost::mpl::less, mpl_::void_>::apply<boost::integral_constant<long unsigned int, 8ul>, boost::integral_constant<long unsigned int, 1ul> >'
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/apply_wrap.hpp:49:   instantiated from 'boost::mpl::apply_wrap2<boost::mpl::quote2<boost::mpl::less, mpl_::void_>, boost::integral_constant<long unsigned int, 8ul>, boost::integral_constant<long unsigned int, 1ul> >'
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/bind.hpp:207:   instantiated from 'boost::mpl::bind2<boost::mpl::quote2<boost::mpl::less, mpl_::void_>, mpl_::arg<-0x00000000000000001>, mpl_::arg<-0x00000000000000001> >::apply<boost::integral_constant<long unsigned int, 8ul>, boost::integral_constant<long unsigned int, 1ul>, mpl_::na, mpl_::na, mpl_::na>'
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/apply_wrap.hpp:49:   instantiated from 'boost::mpl::apply_wrap2<boost::mpl::protect<boost::mpl::bind2<boost::mpl::quote2<boost::mpl::less, mpl_::void_>, mpl_::arg<-0x00000000000000001>, mpl_::arg<-0x00000000000000001> >, 0>, boost::integral_constant<long unsigned int, 8ul>, boost::integral_constant<long unsigned int, 1ul> >'
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/apply.hpp:73:   instantiated from 'boost::mpl::apply2<boost::mpl::less<mpl_::arg<-0x00000000000000001>, mpl_::arg<-0x00000000000000001> >, boost::integral_constant<long unsigned int, 8ul>, boost::integral_constant<long unsigned int, 1ul> >'
/usr/local/include/boost/mpl/max_element.hpp:42:   instantiated from 'boost::mpl::aux::select_max<boost::mpl::less<mpl_::arg<-0x00000000000000001>, mpl_::arg<-0x00000000000000001> > >::apply<boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<6l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<5l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<4l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<3l>, boost::integral_constant<long unsigned int, 1ul>, boost::mpl::l_item<mpl_::long_<2l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<1l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_end> > > > > > >, boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<3l>, boost::integral_constant<long unsigned int, 1ul>, boost::mpl::l_item<mpl_::long_<2l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<1l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_end> > > > >'
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/apply_wrap.hpp:49:   instantiated from 'boost::mpl::apply_wrap2<boost::mpl::protect<boost::mpl::aux::select_max<boost::mpl::less<mpl_::arg<-0x00000000000000001>, mpl_::arg<-0x00000000000000001> > >, 0>, boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<6l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<5l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<4l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<3l>, boost::integral_constant<long unsigned int, 1ul>, boost::mpl::l_item<mpl_::long_<2l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<1l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_end> > > > > > >, boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<3l>, boost::integral_constant<long unsigned int, 1ul>, boost::mpl::l_item<mpl_::long_<2l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<1l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_end> > > > >'
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/apply.hpp:73:   instantiated from 'boost::mpl::apply2<boost::mpl::protect<boost::mpl::aux::select_max<boost::mpl::less<mpl_::arg<-0x00000000000000001>, mpl_::arg<-0x00000000000000001> > >, 0>, boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<6l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<5l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<4l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<3l>, boost::integral_constant<long unsigned int, 1ul>, boost::mpl::l_item<mpl_::long_<2l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<1l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_end> > > > > > >, boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<3l>, boost::integral_constant<long unsigned int, 1ul>, boost::mpl::l_item<mpl_::long_<2l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<1l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_end> > > > >'
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/iter_fold_impl.hpp:115:   instantiated from 'boost::mpl::aux::iter_fold_impl<4, boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<6l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<5l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<4l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<3l>, boost::integral_constant<long unsigned int, 1ul>, boost::mpl::l_item<mpl_::long_<2l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<1l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_end> > > > > > >, boost::mpl::l_iter<boost::mpl::l_end>, boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<6l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<5l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<4l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<3l>, boost::integral_constant<long unsigned int, 1ul>, boost::mpl::l_item<mpl_::long_<2l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<1l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_end> > > > > > >, boost::mpl::protect<boost::mpl::aux::select_max<boost::mpl::less<mpl_::arg<-0x00000000000000001>, mpl_::arg<-0x00000000000000001> > >, 0> >'
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/iter_fold_impl.hpp:146:   instantiated from 'boost::mpl::aux::iter_fold_impl<6, boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<6l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<5l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<4l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<3l>, boost::integral_constant<long unsigned int, 1ul>, boost::mpl::l_item<mpl_::long_<2l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<1l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_end> > > > > > >, boost::mpl::l_iter<boost::mpl::l_end>, boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<6l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<5l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<4l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<3l>, boost::integral_constant<long unsigned int, 1ul>, boost::mpl::l_item<mpl_::long_<2l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<1l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_end> > > > > > >, boost::mpl::protect<boost::mpl::aux::select_max<boost::mpl::less<mpl_::arg<-0x00000000000000001>, mpl_::arg<-0x00000000000000001> > >, 0> >'
/usr/local/include/boost/mpl/iter_fold.hpp:40:   instantiated from 'boost::mpl::iter_fold<boost::mpl::l_item<mpl_::long_<6l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<5l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<4l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<3l>, boost::integral_constant<long unsigned int, 1ul>, boost::mpl::l_item<mpl_::long_<2l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<1l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_end> > > > > >, boost::mpl::l_iter<boost::mpl::l_item<mpl_::long_<6l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<5l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<4l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<3l>, boost::integral_constant<long unsigned int, 1ul>, boost::mpl::l_item<mpl_::long_<2l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<1l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_end> > > > > > >, boost::mpl::protect<boost::mpl::aux::select_max<boost::mpl::less<mpl_::arg<-0x00000000000000001>, mpl_::arg<-0x00000000000000001> > >, 0> >'
/usr/local/include/boost/mpl/max_element.hpp:65:   instantiated from 'boost::mpl::max_element<boost::mpl::l_item<mpl_::long_<6l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<5l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<4l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<3l>, boost::integral_constant<long unsigned int, 1ul>, boost::mpl::l_item<mpl_::long_<2l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_item<mpl_::long_<1l>, boost::integral_constant<long unsigned int, 8ul>, boost::mpl::l_end> > > > > >, boost::mpl::less<mpl_::arg<-0x00000000000000001>, mpl_::arg<-0x00000000000000001> > >'
/usr/local/include/boost/variant/variant.hpp:123:   instantiated from 'boost::detail::variant::max_value<boost::mpl::l_item<mpl_::long_<6l>, std::basic_string<char, std::char_traits<char>, std::allocator<char> >, boost::mpl::l_item<mpl_::long_<5l>, boost::recursive_wrapper<std::vector<json_spirit::Pair_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > >, std::allocator<json_spirit::Pair_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > > > > >, boost::mpl::l_item<mpl_::long_<4l>, boost::recursive_wrapper<std::vector<json_spirit::Value_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > >, std::allocator<json_spirit::Value_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > > > > >, boost::mpl::l_item<mpl_::long_<3l>, bool, boost::mpl::l_item<mpl_::long_<2l>, long int, boost::mpl::l_item<mpl_::long_<1l>, double, boost::mpl::l_end> > > > > >, boost::alignment_of<mpl_::arg<1> > >'
/usr/local/include/boost/variant/variant.hpp:238:   instantiated from 'boost::detail::variant::make_storage<boost::mpl::l_item<mpl_::long_<6l>, std::basic_string<char, std::char_traits<char>, std::allocator<char> >, boost::mpl::l_item<mpl_::long_<5l>, boost::recursive_wrapper<std::vector<json_spirit::Pair_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > >, std::allocator<json_spirit::Pair_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > > > > >, boost::mpl::l_item<mpl_::long_<4l>, boost::recursive_wrapper<std::vector<json_spirit::Value_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > >, std::allocator<json_spirit::Value_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > > > > >, boost::mpl::l_item<mpl_::long_<3l>, bool, boost::mpl::l_item<mpl_::long_<2l>, long int, boost::mpl::l_item<mpl_::long_<1l>, double, boost::mpl::l_end> > > > > >, boost::variant<std::basic_string<char, std::char_traits<char>, std::allocator<char> >, boost::recursive_wrapper<std::vector<json_spirit::Pair_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > >, std::allocator<json_spirit::Pair_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > > > > >, boost::recursive_wrapper<std::vector<json_spirit::Value_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > >, std::allocator<json_spirit::Value_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > > > > >, bool, long int, double, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_>::has_fallback_type_>'
/usr/local/include/boost/variant/variant.hpp:1098:   instantiated from 'boost::variant<std::basic_string<char, std::char_traits<char>, std::allocator<char> >, boost::recursive_wrapper<std::vector<json_spirit::Pair_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > >, std::allocator<json_spirit::Pair_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > > > > >, boost::recursive_wrapper<std::vector<json_spirit::Value_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > >, std::allocator<json_spirit::Value_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > > > > >, bool, long int, double, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_, boost::detail::variant::void_>'
json/json_spirit_value.h:89:   instantiated from 'json_spirit::Value_impl<json_spirit::Config_vector<std::basic_string<char, std::char_traits<char>, std::allocator<char> > > >'
rpc.cpp:34:   instantiated from here
/usr/local/include/boost/mpl/aux_/preprocessed/gcc/less.hpp:90: warning: comparison between 'enum mpl_::integral_c<long unsigned int, 1ul>::<anonymous>' and 'enum mpl_::integral_c<long unsigned int, 8ul>::<anonymous>'
g++ -c -O0 -Wno-invalid-offsetof -Wformat -g -D__WXDEBUG__ -D__WXGTK__ -DNOPCH -I"/usr/include" -I"/usr/local/include/wx-2.9" -I"/usr/local/lib/wx/include/gtk2-unicode-release-2.9" -I"/usr/local/include/db48" -I"/usr/local/include"  -DwxUSE_GUI=0 -o obj/nogui/init.o init.cpp
g++ -c -O0 -Wno-invalid-offsetof -Wformat -g -D__WXDEBUG__ -D__WXGTK__ -DNOPCH -I"/usr/include" -I"/usr/local/include/wx-2.9" -I"/usr/local/lib/wx/include/gtk2-unicode-release-2.9" -I"/usr/local/include/db48" -I"/usr/local/include"  -O3 -o obj/sha.o sha.cpp
g++ -O0 -Wno-invalid-offsetof -Wformat -g -D__WXDEBUG__ -D__WXGTK__ -DNOPCH -I"/usr/include" -I"/usr/local/include/wx-2.9" -I"/usr/local/lib/wx/include/gtk2-unicode-release-2.9" -I"/usr/local/include/db48" -I"/usr/local/include"  -o bitcoind -L"/usr/lib" -L"/usr/local/lib" -L"/usr/local/lib/db48" obj/nogui/util.o obj/nogui/script.o obj/nogui/db.o obj/nogui/net.o obj/nogui/irc.o obj/nogui/main.o obj/nogui/rpc.o obj/nogui/init.o obj/sha.o -l wx_baseu-2.9 -Wl,-Bstatic -l boost_system -l boost_filesystem -l db_cxx -Wl,-Bdynamic -l crypto -l gthread-2.0
obj/nogui/init.o(.gnu.linkonce.t._ZNK13wxArrayString4ItemEm+0x13): In function `wxArrayString::Item(unsigned long) const':
/usr/local/include/wx-2.9/wx/buffer.h:42: undefined reference to `wxTheAssertHandler'
obj/nogui/init.o(.gnu.linkonce.t._ZNK13wxArrayString4ItemEm+0x45): In function `wxArrayString::Item(unsigned long) const':
/usr/src/bitcoin/trunk/uint256.h:526: undefined reference to `wxOnAssert(char const*, int, char const*, char const*, wchar_t const*)'
gmake: *** [bitcoind] Error 1
This is on FreeBSD 7.2.  Any ideas?

---

## Post #5

**Author:** Minsc

**Date:** April 21, 2010, 11:04:52 PM

Re: Is there a way to automate bitcoin payments for a website?
April 21, 2010, 11:04:52 PM
#5
I don't even understand what madhatter2 said.
It'd be nice to have just one single port to install and then I could just leave it as a terminate and stay resident program.  Then it would log payments received in a file and could be communicated with via shell commands.

---

## Post #6

**Author:** Link2VoIP

**Date:** April 22, 2010, 12:31:46 AM

Re: Is there a way to automate bitcoin payments for a website?
April 22, 2010, 12:31:46 AM
#6
You are partly correct. bitcoind is a "terminate and stay resident program". You communicate with it over a JSON-RPC socket to send payments/poll for received payments.

---

## Post #7

**Author:** satoshi

**Date:** May 18, 2010, 02:58:11 AM

Re: Is there a way to automate bitcoin payments for a website?
May 18, 2010, 02:58:11 AM
#7
A little late, but in case anyone else has the same issue.  The compile dump had 2 warnings (that were 20 lines long) and 2 link errors.  The errors were:
Quote
obj/nogui/init.o(.gnu.linkonce.t._ZNK13wxArrayString4ItemEm+0x13): In function `wxArrayString::Item(unsigned long) const':
/usr/local/include/wx-2.9/wx/buffer.h:42: undefined reference to `wxTheAssertHandler'
obj/nogui/init.o(.gnu.linkonce.t._ZNK13wxArrayString4ItemEm+0x45): In function `wxArrayString::Item(unsigned long) const':
/usr/src/bitcoin/trunk/uint256.h:526: undefined reference to `wxOnAssert(char const*, int, char const*, char const*, wchar_t const*)'
Those are probably due to switching to the release build of wxWidgets instead of debug.  They're moving towards only debug build and ditching the release build, so they probably don't care that their release build is broken by referring to non-existent assert stuff.  There's nothing to fear about the debug build.  It's fully suitable for releases.
bitcoind runs as a daemon and can either be controlled by command line or JSON-RPC.
Thanks madhatter and generica for detailing the instructions for building on freebsd.

---

## Post #8

**Author:** tola

**Date:** March 07, 2017, 12:06:35 PM

Re: Is there a way to automate bitcoin payments for a website?
March 07, 2017, 12:06:35 PM
#8
BAKEDcoin
hat horse Boyarsky

---

