# curl-shell-bash-variable
curl命令使用变量，引号""内 $参数 use variable in curl bash commond

```shell
%%shell
token="eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..."
curl_dtr=$(curl 'https://apigateway.muvi.com/content' \
  -H 'authority: apigateway.muvi.com' \
  -H 'accept: application/json, text/plain, */*' \
  -H 'accept-language: zh-CN,zh;q=0.9' \
  -H "authorization: Bearer $token" \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -H 'user-agent: Mozilla/5.0 (Linux; Android 10; K) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0.0.0 Mobile Safari/537.36' \
  --data-raw '{"query":"{featuredContentList(user_type:\":user_type\",end_user_uuid: \":me\",ip_address:\":ip\",app_token: \":app_token\", product_key: \":product_key\", store_key:\":store_key\", user_uuid:\":me\", feature_section_uuid:\"b7cc8ddfe78d4bddbfb12d95d297818f\",page_content:1,per_page_content:12,content_asset_type:\"\", maturity_rating_uuid: \":maturity_rating_uuid\", profile_uuid:\":profile_uuid\"){featured_content_list {feature_section_type feature_section_name feature_section_uuid  section_content_list {page_info{total_count} content_list{document_details{document_uuid file_url original_file_name file_name} maturity_rating content_created_by user_type  is_encoded is_playlist is_parent app_token product_key content_name is_free_content content_permalink content_search_tag content_trailer_uuid content_uuid content_parent_uuid content_desc content_asset_type content_asset_uuid no_image_available_url content_level categories{ category_name } trailer_details {video_uuid file_name file_url} video_details {is_feed  encoding_status encoding_end_time file_name  third_party_url } audio_details {audio_uuid encoding_status file_name  } posters {website{file_uuid file_url file_name} tv_apps{file_uuid file_url file_name} mobile_apps{file_uuid file_url file_name} } banners {website{file_uuid file_url file_name} tv_apps{file_uuid file_url file_name} mobile_apps{file_uuid file_url file_name}} }}}}}"}' \
  --compressed)

echo $curl_dtr
echo $curl_dtr | jq -r '.data.featuredContentList.featured_content_list[].section_content_list.content_list[].posters.website[].file_url' | xargs -n 1 curl -O

echo $curl_dtr | jq -r '.data.featuredContentList.featured_content_list[].section_content_list.content_list[].posters.mobile_apps[].file_url' | xargs -n 1 curl -O
```

使用双引号， curl_output=$(curl x.com "$token")包裹单行或者多行命令，然后echo $curl_output打印

```shell
ok="okkk"
curl_output=$(curl https://baidu.com \
-H "referer: $ok")
echo $curl_output
echo $curl_output | jq
```
这种方法，echo用于打印结果
可以用`echo $curl_output | jq`方便看

```shell
%%shell

value="https://baidu.com/"
curl_output=$(curl --location 
  --request POST "$value" 
  --header "Content-Type: application/json" 
  --header "id: $empid" 
  --data-raw '{
    "clientName": "name",
    "role": "dev",
    "group": "groupid"
  }')

echo "$curl_output"
```

```bash
%%shell
token="eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9SHvXTxY71kxTPcJlLOO_ceSAPRtyccTpbETeV8f1wZ4"
curl_dtr=$(curl -s --location 'https://apigateway.muvi.com/content' \
  -H 'authority: apigateway.muvi.com' \
  -H 'accept: application/json, text/plain, */*' \
  -H 'accept-language: zh-CN,zh;q=0.9' \
  -H "authorization: Bearer $token" \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -H 'origin: https://nymey.com' \
  -H 'pragma: no-cache' \
  -H 'referer: https://nymey.com/' \
  -H 'sec-ch-ua: "Not-A.Brand";v="99", "Chromium";v="124"' \
  -H 'sec-ch-ua-mobile: ?1' \
  -H 'sec-ch-ua-platform: "Android"' \
  -H 'sec-fetch-dest: empty' \
  -H 'sec-fetch-mode: cors' \
  -H 'sec-fetch-site: cross-site' \
  -H 'user-agent: Mozilla/5.0 (Linux; Android 10; K) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0.0.0 Mobile Safari/537.36' \
  --data-raw '{"query":"{featuredContentList(user_type:\":user_type\",end_user_uuid: \":me\",ip_address:\":ip\",app_token: \":app_token\", product_key: \":product_key\", store_key:\":store_key\", user_uuid:\":me\", feature_section_uuid:\"b7cc8ddfe78d4bddbfb12d95d297818f\",page_content:3,per_page_content:12,content_asset_type:\"\", maturity_rating_uuid: \":maturity_rating_uuid\", profile_uuid:\":profile_uuid\"){featured_content_list {feature_section_type feature_section_name feature_section_uuid  section_content_list {page_info{total_count} content_list{document_details{document_uuid file_url original_file_name file_name} maturity_rating content_created_by user_type  is_encoded is_playlist is_parent app_token product_key content_name is_free_content content_permalink content_search_tag content_trailer_uuid content_uuid content_parent_uuid content_desc content_asset_type content_asset_uuid no_image_available_url content_level categories{ category_name } trailer_details {video_uuid file_name file_url} video_details {is_feed  encoding_status encoding_end_time file_name  third_party_url } audio_details {audio_uuid encoding_status file_name  } posters {website{file_uuid file_url file_name} tv_apps{file_uuid file_url file_name} mobile_apps{file_uuid file_url file_name} } banners {website{file_uuid file_url file_name} tv_apps{file_uuid file_url file_name} mobile_apps{file_uuid file_url file_name}} }}}}}"}' \
  --compressed)

echo "$curl_dtr" 
echo "$curl_dtr" | jq -r '.data.featuredContentList.featured_content_list[].section_content_list.content_list[].posters | {
    mobile_image: .mobile_apps[0].file_url,
    website_image: .website[0].file_url
}' | jq -r '(.mobile_image, .website_image) | @text' | xargs -n 2 sh -c '
    for url; do
        wget -P /content/ "$url" -O "$(basename "$url")"
    done
'
```

```
%%shell

 
echo "$curl_dtr"
# 然后使用 wget 下载每个链接的图片
echo "$curl_dtr" | jq -r '.data.featuredContentList.featured_content_list[] | .section_content_list.content_list[] | {
    uuid: .content_uuid,
    permalink: .content_permalink,
    video: .trailer_details.file_url,
    mobile_image: .posters.mobile_apps[0].file_url,
    website_image: .posters.website[0].file_url
}'
```

查看响应示例，会直接打印：

```json
{
  "uuid": "05099607cc1a4806a038d648d1e761f0",
  "permalink": "that-time-i-got-reincarnated-as-a-slime",
  "video": null,
  "mobile_image": "https://d10xsoss226fg9.cloudfront.net/cN6YB0fU2ColhHGAv5NIjVGtN61G6zsU/3D28C83C85544F0D9517DD2E7C7EEDAA/il/e83659cff5d2464eab4218e873b86c32/b937bb468eb142c59805ca515d57a6da/mobileposters/_______________________EN-1732103964002-1732103997284.jpg",
  "website_image": "https://d10xsoss226fg9.cloudfront.net/cN6YB0fU2ColhHGAv5NIjVGtN61G6zsU/3D28C83C85544F0D9517DD2E7C7EEDAA/il/03869e8353314436902244f70f193d1a/e7e694e08e48446d83bdb6dcfdd67c05/webposters/_______________________EN-1732103963994.jpg"
}
{
  "uuid": "687c5bd8293643beadcb2af5f50679f5",
  "permalink": "a-nobodys-way-up-to-an-exploration-hero",
…………
```


