<template lang="html">
    <StackLayout v-if="data.placeBookmarkList.length>0" @swipe="onSwipe" >
        <AbsoluteLayout>
            <ListView for="placeBookmark in data.placeBookmarkList" separatorColor="transparent"
                      style="height:100%"  @swipe="onSwipe" marginTop="20">
                <v-template>
                    <StackLayout class="storeMarkDetailSubWrap" @swipe="onSwipe" @tap="goPlaceDetail(placeBookmark)" >
                        <StackLayout orientation="horizontal" class="storeMarkDetailWrap"  v-shadow="{ elevation: 2,shape:'RECTANGLE', bgcolor: 'white', cornerRadius: 10 }">
                            <StackLayout class="storeLeftWrap" v-if="placeBookmark.thumbnail != ''">
                                <image class="storeImage" stretch="aspectFill" :src="placeBookmark.thumbnail"/>
                            </StackLayout>
                            <StackLayout class="storeLeftWrap" v-else backgroundColor="#dddddd" borderRadius="10">
                                <image class="storeImage" width="55" height="55" marginTop="8" stretch="aspectFill" src="~/Resources/img/home/dinner_w_64.png"/>
                            </StackLayout>
                            <StackLayout class="storeMarkRightWrap">
                                <StackLayout orientation="horizontal" class="storeMarkTopWrap">
                                    <StackLayout class="storeMarkTimeWrap" v-if="placeBookmark.open_hours !=''">
                                        <label :text="openingCheck(placeBookmark.open_hours)" class="storeTime" />
                                    </StackLayout>
                                    <StackLayout class="storeMarkTimeWrap" v-else>
                                        <label text="정보없음" class="storeTime" />
                                    </StackLayout>
                                    <StackLayout class="storeStarIconWrap">
                                        <Image src="~/Resources/img/home/star.png" class="storeStarIcon" />
                                    </StackLayout>
                                    <StackLayout class="storeRatingWrap" v-if="placeBookmark.rating_point != ''">
<!--                                        <label :text="placeBookmark.rating_point" class="storeRating"/>-->
                                        <label :text="placeBookmark.google_rating" v-if="placeBookmark.app_rating < 1&&placeBookmark.google_rating > 0"  class="storeRating"/>
                                        <label :text="placeBookmark.app_rating" v-else-if="placeBookmark.app_rating > 0"  class="storeRating"/>
                                        <label text="0" v-else class="storeRating"/>
                                    </StackLayout>
                                    <StackLayout class="storeRatingWrap" v-else >
                                        <label text="0" class="storeRating"/>
                                    </StackLayout>
                                </StackLayout>
                                <StackLayout class="storeMarkMiddleWrap">
                                    <label :text="placeBookmark.place_name" class="storeMarkName"/>
                                </StackLayout>
                                <StackLayout orientation="horizontal" class="storeMarkBottomWrap">
                                    <StackLayout class="storeMarkCategoryWrap">
                                        <label :text="placeBookmark.category_name" class="storeMarkCategory"/>
                                    </StackLayout>
                                    <StackLayout class="storeMarkYIconWrap">
                                        <image v-if="placeBookmark.youtube_review_count>0" class="storeMarkYIcon" stretch="aspectFill" src="~/Resources/img/bookmark/youtube-circle.png"/>
                                        <image v-else class="storeMarkYIcon" stretch="aspectFill" src="~/Resources/img/bookmark/youtube-circle_g.png"/>
<!--                                        <image class="storeMarkYIcon" stretch="aspectFill" src="~/Resources/img/bookmark/youtube-circle.png"/>-->
                                    </StackLayout>
                                    <StackLayout class="storeMarkNIconWrap">
                                        <image v-if="placeBookmark.naver_blog_count > 0" class="storeMarkNIcon"stretch="aspectFill"  src="~/Resources/img/bookmark/naver-circle.png"/>
                                        <image v-else class="storeMarkNIcon"stretch="aspectFill"  src="~/Resources/img/bookmark/naver-circle_g.png"/>
<!--                                        <image class="storeMarkNIcon"stretch="aspectFill"  src="~/Resources/img/bookmark/naver-circle.png"/>-->
                                    </StackLayout>
                                    <StackLayout class="storeMarkTIconWrap">
                                        <image  v-if="placeBookmark.daum_blog_count > 0" class="storeMarkTIcon" stretch="aspectFill" src="~/Resources/img/bookmark/tistory-circle.png"/>
                                        <image v-else class="storeMarkTIcon" stretch="aspectFill" src="~/Resources/img/bookmark/tistory-circle_g.png"/>
<!--                                        <image class="storeMarkTIcon" stretch="aspectFill" src="~/Resources/img/bookmark/tistory-circle.png"/>-->
                                    </StackLayout>
                                    <StackLayout v-if="placeBookmark.google_review_count > 0" class="storeMarkGIconWrap"  v-shadow="{ elevation: 2,shape:'RECTANGLE', bgcolor: 'white', cornerRadius: 50 }">
                                        <image class="storeMarkGIcon"stretch="aspectFill" src="~/Resources/img/bookmark/google-circle.png" />
                                    </StackLayout>
                                    <StackLayout v-else class="storeMarkGIconWrap"  v-shadow="{ elevation: 2,shape:'RECTANGLE', bgcolor: '#888888', cornerRadius: 50 }">
                                        <image  class="storeMarkGIcon"stretch="aspectFill" src="~/Resources/img/bookmark/google-circle_g.png" />
                                    </StackLayout>
<!--                                    <StackLayout class="storeMarkGIconWrap"  v-shadow="{ elevation: 2,shape:'RECTANGLE', bgcolor: 'white', cornerRadius: 50 }">-->
<!--                                        <image class="storeMarkGIcon"stretch="aspectFill" src="~/Resources/img/bookmark/google-circle.png" />-->
<!--                                    </StackLayout>-->
                                    <StackLayout class="storeMarkAIconWrap" >
                                        <image class="storeMarkAIcon" stretch="aspectFill"  src="~/Resources/img/place/playstore-icon.png" />
                                    </StackLayout>
                                </StackLayout>
                            </StackLayout>
                        </StackLayout>
                        <StackLayout class="markUnderline">
                        </StackLayout>
                    </StackLayout>
                </v-template>
            </ListView>
            <StackLayout width="100%" height="100%" backgroundColor="#dddddd" top="0" opacity="0.4" v-if="busy==true">

            </StackLayout>
            <StackLayout top="0" width="100%">
                <ActivityIndicator :busy="busy" marginTop="230" color="#ffe074" width="100" height="100" />
            </StackLayout>
        </AbsoluteLayout>

    </StackLayout>
    <StackLayout v-else  @swipe="onSwipe">
        <AbsoluteLayout>
            <StackLayout width="100%" style="text-align: center">
                <label text="저장 된 내용이 없습니다." fontSize="14" color="#333333" marginTop="300" fontFamily="nanumsquareroundr" />
            </StackLayout>
            <StackLayout width="100%" height="100%" backgroundColor="#dddddd" top="0" opacity="0.4" v-if="busy==true">

            </StackLayout>
            <StackLayout top="0" width="100%">
                <ActivityIndicator :busy="busy" marginTop="230" color="#ffe074" width="100" height="100" />
            </StackLayout>
        </AbsoluteLayout>
    </StackLayout>
</template>
<script>
 import axios from 'axios';


 //import TistoryWebview from '../../search/place/placeDetail/reviewMore/reviewMoreWebview/TistoryWebview'

 import '~/Resources/css/menu/bookmark/bookmarkList/PlaceBookmark/placeBookmark_320.scss';
 import '~/Resources/css/menu/bookmark/bookmarkList/PlaceBookmark/placeBookmark_360.scss';
 import '~/Resources/css/menu/bookmark/bookmarkList/PlaceBookmark/placeBookmark_420.scss';
 import '~/Resources/css/menu/bookmark/bookmarkList/PlaceBookmark/placeBookmark_480.scss';

 let data = {placeBookmarkList:[]};
 const frameModule = require("ui/frame"); // #A
 const appSettings = require("tns-core-modules/application-settings");
 const enums = require("tns-core-modules/ui/enums");
 const platformModule = require("tns-core-modules/platform");
 // other imports here
 import * as application from "application";
 import * as frame from "ui/frame";
 import PlaceDetail from "../../search/place/placeDetail/PlaceDetail";
 var cache = require("nativescript-cache");

 export default {
        name:"PlaceBookmark",
        data(){
            return {
                data,
                busy:false,
            }
        },
        components: {
        },mounted() {
            this.getPlaceBookmark();


         //
         // setTimeout(() => {
         //     while(true){
         //         this.$refs.aaa.nativeView
         //             .animate({
         //                 translate: {x: 0, y: 100},
         //                 duration: 1000,
         //                 curve: enums.AnimationCurve.easeIn
         //             })
         //
         //         this.$refs.aaa.nativeView
         //             .animate({
         //                 translate: {x: 100, y: 0},
         //                 duration: 1000,
         //                 curve: enums.AnimationCurve.easeIn
         //             })
         //     }
         // }, 1000);


     },created(){
        // this.getPlaceBookmark();
     },methods: {

         goPlaceDetail(args){
            console.log(args)
            //const view = args.view;
            //const tappedItem = view.bindingContext;
             //data.keyword = tappedItem;
             //this.placeSearch(data.keyword);
             //this.$data.listViewing = 'place';


             cache.set('place_id',args.place_id)
             cache.set('place_name',args.place_name)
             cache.set('place_address',args.place_address);
             this.$navigateTo(PlaceDetail, {
                 props: {
                     context: args}});
         },
            onSwipe(args){

                console.log('wwdasda')
                console.log("Swipe!");
                console.log("Object that triggered the event: " + args.object);
                console.log("View that triggered the event: " + args.view);
                console.log("Event name: " + args.eventName);
                console.log("Swipe Direction: " + args.direction);

                this.getPlaceBookmark();
                // this.$data.busy = true;
                // setTimeout(() => {
                //     axios({
                //         method: 'get',
                //         url: 'http://api.eatjeong.com/v1/bookmarks',
                //         params: {
                //             gubun: 'place',
                //             user_id : appSettings.getString("user_id"),
                //             sns_division:appSettings.getString("sns_division")
                //         },
                //     }).then((response) => {
                //         this.$data.busy = false;
                //         console.log(response.data.dataList)
                //         console.log("zzzzzzzzzz"+response.data.dataList);
                //         this.data.placeBookmarkList = response.data.dataList;
                //     }, (error) => {
                //         this.$data.busy = false;
                //         console.log(error);
                //     });
                // }, 1000);
            },
            aa(){
               console.log('123123123123123123123')
            },

            getPlaceBookmark(){
                this.$data.busy = true;
                // after class code:
                setTimeout(() => {
                    axios({
                        method: 'get',
                        url: 'http://api.eatjeong.com/v1/bookmarks',
                        params: {
                            gubun: 'place',
                            user_id : appSettings.getString("user_id"),
                            sns_division:appSettings.getString("sns_division")
                        },
                    }).then((response) => {
                        this.$data.busy = false;
                        console.log(response.data.dataList)
                        console.log("zzzzzzzzzz"+response.data.dataList.length + "asdasd");
                        this.data.placeBookmarkList = response.data.dataList;
                    }, (error) => {
                        this.$data.busy = false;
                        console.log(error);
                    });
                }, 1000);

            },openingCheck(open_hours){
                try{
                    var date = new Date();
                    var day = date.toDateString().split(" ")[0];
                    var before_day;
                    var time ;
                    var hour;
                    var minute;
                    var dayList = ['Mon','Tue','Wed','Thu','Fri','Sat','Sun'];
                    var opening_hours = JSON.parse(open_hours);

                    //구글에서아예 서치를 못해 온 경우
                    if (opening_hours == null){
                        return "정보없음";
                    }else{
                        //오늘 날짜에 대한 정보 초기화
                        var close_time;
                        var open_time;
                        var close_nextday_flag;

                        //이전 날짜에 대한 정보 초기화
                        var before_nextday_flag;
                        var before_close_time;

                        //이전 날짜 확인
                        if(day == "Mon"){
                            before_day = dayList[dayList.indexOf(day)+6];
                        }else {
                            before_day = dayList[dayList.indexOf(day)-1];
                        }

                        //오늘 날짜 체크를위한 데이터 추가
                        close_time = opening_hours[day].close;
                        open_time = opening_hours[day].open;
                        close_nextday_flag = opening_hours[day].close_nextday;

                        //이전 날짜 체크를위한 데이터 추가
                        before_nextday_flag =  opening_hours[before_day].close_nextday;
                        before_close_time = opening_hours[before_day].close;


                        //현재의 시간 세팅
                        if(date.getHours().toString().length == 1){
                            hour = "0" + date.getHours().toString();
                        }else{
                            hour = date.getHours().toString();
                        }
                        if(date.getMinutes().toString().length == 1){
                            minute = "0" + date.getMinutes().toString();
                        }else {
                            minute = date.getMinutes().toString();
                        }
                        time = hour + minute; //접속 된 현재 시간 체크

                        if(before_nextday_flag == 'Y'){
                            if(time < before_close_time){
                                return "영업중";
                            }else{
                                if(open_time == ""){
                                    return "휴무"
                                }else{
                                    if(close_nextday_flag == 'Y'){
                                        close_time = parseInt(close_time) + 2400;
                                    }
                                    if(time < open_time ){
                                        return "영업종료"
                                    }else{
                                        if(time > close_time){
                                            return "영업종료"
                                        }else{
                                            return "영업중"
                                        }
                                    }
                                }
                            }
                        }else{
                            if(open_time == ""){
                                return "영업종료"
                            }else{
                                if(close_nextday_flag == 'Y'){
                                    close_time = parseInt(close_time) + 2400;
                                }
                                if(time < open_time ){
                                    return "영업종료"
                                }else{
                                    if(time > close_time){
                                        return "영업종료"
                                    }else{
                                        return "영업중"
                                    }
                                }
                            }
                        }
                    }
                }catch (e) {
                    console.log(e)
                }

            }
        }
    };
</script>

<style lang="scss">


</style>

