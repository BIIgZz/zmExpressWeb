<template>
  <div class="account-settings-info-view" style='margin-left: 100px;margin-top: 20px '>
    <a-row :gutter="16">
      <a-col :md="24" :lg="16">
        <a-form-model layout="vertical"  :model="formData"  ref="ruleForm">
          <a-form-model-item
            label="用户类型"
            :required="true"
            style="margin-bottom:-5px"
          >
            <a-select
              v-model='formData.type' placeholder="请选择用户类型"  default-value = "enterprise"
            >
              <a-select-option value="企业" >
                企业
              </a-select-option>
              <a-select-option value="个人">
                个人
              </a-select-option>
            </a-select>

          </a-form-model-item>
          <a-form-model-item
            label="公司名"
            :required="true"
            style="margin-bottom:-5px"
          >
            <a-input
              placeholder="请输入公司名"  v-model='formData.company'
            />
          </a-form-model-item>

          <a-form-model-item
            label="营业执照号码"
            :required="true"
            style="margin-bottom:-5px"
          >
            <a-input placeholder="请输入营业执照号码"  v-model='formData.license'/>
          </a-form-model-item>
          <a-form-model-item
            label="联系人"
            :required="true"
            style="margin-bottom:-5px"
          >
            <a-input placeholder="请输入联系人"  v-model='formData.linkman'/>
          </a-form-model-item>
          <a-form-model-item
            label="联系电话"
            :required="true"
            style="margin-bottom:-5px"
          >
            <a-input  v-model='formData.tel'
                      v-decorator="['mobilephone',{rules: [{validator: this.validateMobile}]}]" placeholder="请输入联系电话"
            />
          </a-form-model-item>
          <a-form-model-item
            label="附件(请上传图片或pdf文件)"
            :required="true"
            style="margin-bottom:-5px"
          >
            <j-image-upload bizPath="scott/pic" prop="imgList"  v-model='formData.appendix'></j-image-upload>
          </a-form-model-item>
          <a-divider/>
          <a-row :gutter="24">
            <a-col :span="12">
              <span>中文地址</span>
              <a-divider />
              <a-form-model-item
                label="联系人"
                :required="true"
                style="margin-bottom:-5px"
              >
                <a-input placeholder="请输入联系人"  v-model='formData.linkman'/>
              </a-form-model-item>
              <a-form-model-item
                label="联系电话"
                :required="true"
                style="margin-bottom:-5px"
              >
                <a-input placeholder="请输入联系电话"  v-model='formData.tel'/>
              </a-form-model-item>
              <a-form-model-item
                label="地址"
                :required="true"
                style="margin-bottom:-5px"
              >
                <a-input placeholder="请输入地址"  v-model='formData.address' @change="toPingying('address')" />
              </a-form-model-item>
              <a-form-model-item
                label="城市"
                :required="true"
                style="margin-bottom:-5px"
              >
                <j-input placeholder="请输入城市"  v-model='formData.city' @change="toPingying('city')" ></j-input>
              </a-form-model-item>
              <a-form-model-item
                label="省/州"
                :required="true"
                style="margin-bottom:-5px"
              >
                <j-input placeholder="请输入省/州"  v-model='formData.state' @change="toPingying('state')"></j-input>
              </a-form-model-item>
              <a-form-model-item
                label="国家"
                :required="true"
                style="margin-bottom:-5px"
              >
                <a-select placeholder="请输入国家" default-value="CN"  v-model='formData.country'>
                  <a-select-option value="AD">AD-安道尔 Andorra</a-select-option>
                  <a-select-option value="AE">AE-阿联酋 United Arab Emirates</a-select-option>
                  <a-select-option value="AF">AF-阿富汗 Afghanistan</a-select-option>
                  <a-select-option value="AG">AG-安提瓜和巴布达 Antigua And Barbuda</a-select-option>
                  <a-select-option value="AI">AI-安圭拉 Anguilla</a-select-option>
                  <a-select-option value="AL">AL-阿尔巴尼亚 Albania</a-select-option>
                  <a-select-option value="AM">AM-亚美尼亚 Armenia</a-select-option>
                  <a-select-option value="AO">AO-安哥拉 Angola</a-select-option>
                  <a-select-option value="AQ">AQ-南极洲 Antarctica</a-select-option>
                  <a-select-option value="AR">AR-阿根廷 Argentina</a-select-option>
                  <a-select-option value="AS">AS-美属萨摩亚 American Samoa</a-select-option>
                  <a-select-option value="AT">AT-奥地利 Austria</a-select-option>
                  <a-select-option value="AU">AU-澳大利亚 Australia</a-select-option>
                  <a-select-option value="AW">AW-阿鲁巴 Aruba</a-select-option>
                  <a-select-option value="AX">AX-奥兰群岛 Aland Islands</a-select-option>
                  <a-select-option value="AZ">AZ-阿塞拜疆 Azerbaijan</a-select-option>
                  <a-select-option value="BA">BA-波黑 Bosnia And Herzegovina</a-select-option>
                  <a-select-option value="BB">BB-巴巴多斯 Barbados</a-select-option>
                  <a-select-option value="BD">BD-孟加拉国 Bangladesh</a-select-option>
                  <a-select-option value="BE">BE-比利时 Belgium</a-select-option>
                  <a-select-option value="BF">BF-布基纳法索 Burkina Faso</a-select-option>
                  <a-select-option value="BG">BG-保加利亚 Bulgaria</a-select-option>
                  <a-select-option value="BH">BH-巴林 Bahrain</a-select-option>
                  <a-select-option value="BI">BI-布隆迪 Burundi</a-select-option>
                  <a-select-option value="BJ">BJ-贝宁 Benin</a-select-option>
                  <a-select-option value="BL">BL-圣巴泰勒米岛 Saint Barthélemy</a-select-option>
                  <a-select-option value="BM">BM-百慕大 Bermuda</a-select-option>
                  <a-select-option value="BN">BN-文莱 Brunei Darussalam</a-select-option>
                  <a-select-option value="BO">BO-玻利维亚 Bolivia, Plurinational</a-select-option>
                  <a-select-option value="BQ">BQ-荷兰加勒比区 Bonaire, Sint Eustatius And Saba</a-select-option>
                  <a-select-option value="BR">BR-巴西 Brazil</a-select-option>
                  <a-select-option value="BS">BS-巴哈马 Bahamas</a-select-option>
                  <a-select-option value="BT">BT-不丹 Bhutan</a-select-option>
                  <a-select-option value="BV">BV-布韦岛 Bouvet Island</a-select-option>
                  <a-select-option value="BW">BW-博茨瓦纳 Botswana</a-select-option>
                  <a-select-option value="BY">BY-白俄罗斯 Belarus</a-select-option>
                  <a-select-option value="BZ">BZ-伯利兹 Belize</a-select-option>
                  <a-select-option value="CA">CA-加拿大 Canada</a-select-option>
                  <a-select-option value="CC">CC-科科斯群岛 Cocos (keeling) Islands</a-select-option>
                  <a-select-option value="CD">CD-刚果（金） Congo, The Democratic Republic Of The</a-select-option>
                  <a-select-option value="CF">CF-中非 Central African Republic</a-select-option>
                  <a-select-option value="CG">CG-刚果（布） Congo</a-select-option>
                  <a-select-option value="CH">CH-瑞士 Switzerland</a-select-option>
                  <a-select-option value="CI">CI-科特迪瓦 Côte D&#039;ivoire</a-select-option>
                  <a-select-option value="CK">CK-库克群岛 Cook Islands</a-select-option>
                  <a-select-option value="CL">CL-智利 Chile</a-select-option>
                  <a-select-option value="CM">CM-喀麦隆 Cameroon</a-select-option>
                  <a-select-option value="CN" >CN-中国 People&#039;s Republic Of China</a-select-option>
                  <a-select-option value="CO">CO-哥伦比亚 Colombia</a-select-option>
                  <a-select-option value="CR">CR-哥斯达黎加 Costa Rica</a-select-option>
                  <a-select-option value="CU">CU-古巴 Cuba</a-select-option>
                  <a-select-option value="CV">CV-佛得角 Cape Verde</a-select-option>
                  <a-select-option value="CW">CW-库拉索 Curaçao</a-select-option>
                  <a-select-option value="CX">CX-圣诞岛 Christmas Island</a-select-option>
                  <a-select-option value="CY">CY-塞浦路斯 Cyprus</a-select-option>
                  <a-select-option value="CZ">CZ-捷克 Czech Republic</a-select-option>
                  <a-select-option value="DE">DE-德国 Germany</a-select-option>
                  <a-select-option value="DJ">DJ-吉布提 Djibouti</a-select-option>
                  <a-select-option value="DK">DK-丹麦 Denmark</a-select-option>
                  <a-select-option value="DM">DM-多米尼克 Dominica</a-select-option>
                  <a-select-option value="DO">DO-多米尼加 Dominican Republic</a-select-option>
                  <a-select-option value="DZ">DZ-阿尔及利亚 Algeria</a-select-option>
                  <a-select-option value="EC">EC-厄瓜多尔 Ecuador</a-select-option>
                  <a-select-option value="EE">EE-爱沙尼亚 Estonia</a-select-option>
                  <a-select-option value="EG">EG-埃及 Egypt</a-select-option>
                  <a-select-option value="EH">EH-西撒哈拉 Western Sahara</a-select-option>
                  <a-select-option value="ER">ER-厄立特里亚 Eritrea</a-select-option>
                  <a-select-option value="ES">ES-西班牙 Spain</a-select-option>
                  <a-select-option value="ET">ET-埃塞俄比亚 Ethiopia</a-select-option>
                  <a-select-option value="FI">FI-芬兰 Finland</a-select-option>
                  <a-select-option value="FJ">FJ-斐济群岛 Fiji</a-select-option>
                  <a-select-option value="FK">FK-马尔维纳斯群岛（福克兰） Falkland Islands (malvinas)</a-select-option>
                  <a-select-option value="FM">FM-密克罗尼西亚联邦 Micronesia, Federated States Of</a-select-option>
                  <a-select-option value="FO">FO-法罗群岛 Faroe Islands</a-select-option>
                  <a-select-option value="FR">FR-法国 France</a-select-option>
                  <a-select-option value="GA">GA-加蓬 Gabon</a-select-option>
                  <a-select-option value="GB">GB-英国 United Kingdom</a-select-option>
                  <a-select-option value="GD">GD-格林纳达 Grenada</a-select-option>
                  <a-select-option value="GE">GE-格鲁吉亚 Georgia</a-select-option>
                  <a-select-option value="GF">GF-法属圭亚那 French Guiana</a-select-option>
                  <a-select-option value="GG">GG-根西岛 Guernsey</a-select-option>
                  <a-select-option value="GH">GH-加纳 Ghana</a-select-option>
                  <a-select-option value="GI">GI-直布罗陀 Gibraltar</a-select-option>
                  <a-select-option value="GL">GL-格陵兰 Greenland</a-select-option>
                  <a-select-option value="GM">GM-冈比亚 Gambia</a-select-option>
                  <a-select-option value="GN">GN-几内亚 Guinea</a-select-option>
                  <a-select-option value="GP">GP-瓜德罗普 Guadeloupe</a-select-option>
                  <a-select-option value="GQ">GQ-赤道几内亚 Equatorial Guinea</a-select-option>
                  <a-select-option value="GR">GR-希腊 Greece</a-select-option>
                  <a-select-option value="GS">GS-南乔治亚岛和南桑威奇群岛 South Georgia And The South Sandwich Islands</a-select-option>
                  <a-select-option value="GT">GT-危地马拉 Guatemala</a-select-option>
                  <a-select-option value="GU">GU-关岛 Guam</a-select-option>
                  <a-select-option value="GW">GW-几内亚比绍 Guinea-bissau</a-select-option>
                  <a-select-option value="GY">GY-圭亚那 Guyana</a-select-option>
                  <a-select-option value="HK">HK-香港 Hong Kong</a-select-option>
                  <a-select-option value="HM">HM-赫德岛和麦克唐纳群岛 Heard Island And Mcdonald Islands</a-select-option>
                  <a-select-option value="HN">HN-洪都拉斯 Honduras</a-select-option>
                  <a-select-option value="HR">HR-克罗地亚 Croatia</a-select-option>
                  <a-select-option value="HT">HT-海地 Haiti</a-select-option>
                  <a-select-option value="HU">HU-匈牙利 Hungary</a-select-option>
                  <a-select-option value="ID">ID-印尼 Indonesia</a-select-option>
                  <a-select-option value="IE">IE-爱尔兰 Ireland</a-select-option>
                  <a-select-option value="IL">IL-以色列 Israel</a-select-option>
                  <a-select-option value="IM">IM-马恩岛 Isle Of Man</a-select-option>
                  <a-select-option value="IN">IN-印度 India</a-select-option>
                  <a-select-option value="IO">IO-英属印度洋领地 British Indian Ocean Territory</a-select-option>
                  <a-select-option value="IQ">IQ-伊拉克 Iraq</a-select-option>
                  <a-select-option value="IR">IR-伊朗 Iran, Islamic Republic Of</a-select-option>
                  <a-select-option value="IS">IS-冰岛 Iceland</a-select-option>
                  <a-select-option value="IT">IT-意大利 Italy</a-select-option>
                  <a-select-option value="JE">JE-泽西岛 Jersey</a-select-option>
                  <a-select-option value="JM">JM-牙买加 Jamaica</a-select-option>
                  <a-select-option value="JO">JO-约旦 Jordan</a-select-option>
                  <a-select-option value="JP">JP-日本 Japan</a-select-option>
                  <a-select-option value="KE">KE-肯尼亚 Kenya</a-select-option>
                  <a-select-option value="KG">KG-吉尔吉斯斯坦 Kyrgyzstan</a-select-option>
                  <a-select-option value="KH">KH-柬埔寨 Cambodia</a-select-option>
                  <a-select-option value="KI">KI-基里巴斯 Kiribati</a-select-option>
                  <a-select-option value="KM">KM-科摩罗 Comoros</a-select-option>
                  <a-select-option value="KN">KN-圣基茨和尼维斯 Saint Kitts And Nevis</a-select-option>
                  <a-select-option value="KP">KP-朝鲜 Korea, Democratic People&#039;s Republic Of</a-select-option>
                  <a-select-option value="KR">KR-韩国 Korea, Republic Of</a-select-option>
                  <a-select-option value="KV">KV-科索沃 Kosovo</a-select-option>
                  <a-select-option value="KW">KW-科威特 Kuwait</a-select-option>
                  <a-select-option value="KY">KY-开曼群岛 Cayman Islands</a-select-option>
                  <a-select-option value="KZ">KZ-哈萨克斯坦 Kazakhstan</a-select-option>
                  <a-select-option value="LA">LA-老挝 Lao People&#039;s Democratic Republic</a-select-option>
                  <a-select-option value="LB">LB-黎巴嫩 Lebanon</a-select-option>
                  <a-select-option value="LC">LC-圣卢西亚 Saint Lucia</a-select-option>
                  <a-select-option value="LI">LI-列支敦士登 Liechtenstein</a-select-option>
                  <a-select-option value="LK">LK-斯里兰卡 Sri Lanka</a-select-option>
                  <a-select-option value="LR">LR-利比里亚 Liberia</a-select-option>
                  <a-select-option value="LS">LS-莱索托 Lesotho</a-select-option>
                  <a-select-option value="LT">LT-立陶宛 Lithuania</a-select-option>
                  <a-select-option value="LU">LU-卢森堡 Luxembourg</a-select-option>
                  <a-select-option value="LV">LV-拉脱维亚 Latvia</a-select-option>
                  <a-select-option value="LY">LY-利比亚 Libya</a-select-option>
                  <a-select-option value="MA">MA-摩洛哥 Morocco</a-select-option>
                  <a-select-option value="MC">MC-摩纳哥 Monaco</a-select-option>
                  <a-select-option value="MD">MD-摩尔多瓦 Moldova, Republic Of</a-select-option>
                  <a-select-option value="ME">ME-黑山 Montenegro</a-select-option>
                  <a-select-option value="MF">MF-法属圣马丁 Saint Martin (french Part)</a-select-option>
                  <a-select-option value="MG">MG-马达加斯加 Madagascar</a-select-option>
                  <a-select-option value="MH">MH-马绍尔群岛 Marshall Islands</a-select-option>
                  <a-select-option value="MK">MK-马其顿 Macedonia, The Former Yugoslav Republic Of</a-select-option>
                  <a-select-option value="ML">ML-马里 Mali</a-select-option>
                  <a-select-option value="MM">MM-缅甸 Myanmar</a-select-option>
                  <a-select-option value="MN">MN-蒙古国；蒙古 Mongolia</a-select-option>
                  <a-select-option value="MO">MO-澳门 Macao</a-select-option>
                  <a-select-option value="MP">MP-北马里亚纳群岛 Northern Mariana Islands</a-select-option>
                  <a-select-option value="MQ">MQ-马提尼克 Martinique</a-select-option>
                  <a-select-option value="MR">MR-毛里塔尼亚 Mauritania</a-select-option>
                  <a-select-option value="MS">MS-蒙塞拉特岛 Montserrat</a-select-option>
                  <a-select-option value="MT">MT-马耳他 Malta</a-select-option>
                  <a-select-option value="MU">MU-毛里求斯 Mauritius</a-select-option>
                  <a-select-option value="MV">MV-马尔代夫 Maldives</a-select-option>
                  <a-select-option value="MW">MW-马拉维 Malawi</a-select-option>
                  <a-select-option value="MX">MX-墨西哥 Mexico</a-select-option>
                  <a-select-option value="MY">MY-马来西亚 Malaysia</a-select-option>
                  <a-select-option value="MZ">MZ-莫桑比克 Mozambique</a-select-option>
                  <a-select-option value="NA">NA-纳米比亚 Namibia</a-select-option>
                  <a-select-option value="NC">NC-新喀里多尼亚 New Caledonia</a-select-option>
                  <a-select-option value="NE">NE-尼日尔 Niger</a-select-option>
                  <a-select-option value="NF">NF-诺福克岛 Norfolk Island</a-select-option>
                  <a-select-option value="NG">NG-尼日利亚 Nigeria</a-select-option>
                  <a-select-option value="NI">NI-尼加拉瓜 Nicaragua</a-select-option>
                  <a-select-option value="NK">NK-纳戈尔诺-卡拉巴赫 Nagorno-karabakh</a-select-option>
                  <a-select-option value="NL">NL-荷兰 Netherlands</a-select-option>
                  <a-select-option value="NO">NO-挪威 Norway</a-select-option>
                  <a-select-option value="NP">NP-尼泊尔 Nepal</a-select-option>
                  <a-select-option value="NR">NR-瑙鲁 Nauru</a-select-option>
                  <a-select-option value="NU">NU-纽埃 Niue</a-select-option>
                  <a-select-option value="NZ">NZ-新西兰 New Zealand</a-select-option>
                  <a-select-option value="OM">OM-阿曼 Oman</a-select-option>
                  <a-select-option value="PA">PA-巴拿马 Panama</a-select-option>
                  <a-select-option value="PE">PE-秘鲁 Peru</a-select-option>
                  <a-select-option value="PF">PF-法属波利尼西亚 French Polynesia</a-select-option>
                  <a-select-option value="PG">PG-巴布亚新几内亚 Papua New Guinea</a-select-option>
                  <a-select-option value="PH">PH-菲律宾 Philippines</a-select-option>
                  <a-select-option value="PK">PK-巴基斯坦 Pakistan</a-select-option>
                  <a-select-option value="PL">PL-波兰 Poland</a-select-option>
                  <a-select-option value="PM">PM-圣皮埃尔和密克隆 Saint Pierre And Miquelon</a-select-option>
                  <a-select-option value="PN">PN-皮特凯恩群岛 Pitcairn Islands</a-select-option>
                  <a-select-option value="PR">PR-波多黎各 Puerto Rico</a-select-option>
                  <a-select-option value="PS">PS-巴勒斯坦 Palestine, State Of</a-select-option>
                  <a-select-option value="PT">PT-葡萄牙 Portugal</a-select-option>
                  <a-select-option value="PW">PW-帕劳 Palau</a-select-option>
                  <a-select-option value="PY">PY-巴拉圭 Paraguay</a-select-option>
                  <a-select-option value="QA">QA-卡塔尔 Qatar</a-select-option>
                  <a-select-option value="RE">RE-留尼汪 Réunion</a-select-option>
                  <a-select-option value="RO">RO-罗马尼亚 Romania</a-select-option>
                  <a-select-option value="RS">RS-塞尔维亚 Serbia</a-select-option>
                  <a-select-option value="RU">RU-俄罗斯 Russian Federation</a-select-option>
                  <a-select-option value="RW">RW-卢旺达 Rwanda</a-select-option>
                  <a-select-option value="SA">SA-沙特阿拉伯 Saudi Arabia</a-select-option>
                  <a-select-option value="SB">SB-所罗门群岛 Solomon Islands</a-select-option>
                  <a-select-option value="SC">SC-塞舌尔 Seychelles</a-select-option>
                  <a-select-option value="SD">SD-苏丹 Sudan</a-select-option>
                  <a-select-option value="SE">SE-瑞典 Sweden</a-select-option>
                  <a-select-option value="SG">SG-新加坡 Singapore</a-select-option>
                  <a-select-option value="SH">SH-圣赫勒拿 Saint Helena, Ascension And Tristan Da Cunha</a-select-option>
                  <a-select-option value="SI">SI-斯洛文尼亚 Slovenia</a-select-option>
                  <a-select-option value="SJ">SJ-斯瓦尔巴群岛和扬马延岛 Svalbard And Jan Mayen</a-select-option>
                  <a-select-option value="SK">SK-斯洛伐克 Slovakia</a-select-option>
                  <a-select-option value="SL">SL-塞拉利昂 Sierra Leone</a-select-option>
                  <a-select-option value="SM">SM-圣马力诺 San Marino</a-select-option>
                  <a-select-option value="SN">SN-塞内加尔 Senegal</a-select-option>
                  <a-select-option value="SO">SO-索马里 Somalia</a-select-option>
                  <a-select-option value="SR">SR-苏里南 Suriname</a-select-option>
                  <a-select-option value="SS">SS-南苏丹 South Sudan</a-select-option>
                  <a-select-option value="ST">ST-圣多美和普林西比 Sao Tome And Principe</a-select-option>
                  <a-select-option value="SV">SV-萨尔瓦多 El Salvador</a-select-option>
                  <a-select-option value="SX">SX-荷属圣马丁 Sint Maarten (dutch Part)</a-select-option>
                  <a-select-option value="SY">SY-叙利亚 Syrian Arab Republic</a-select-option>
                  <a-select-option value="SZ">SZ-斯威士兰 Swaziland</a-select-option>
                  <a-select-option value="TC">TC-特克斯和凯科斯群岛 Turks And Caicos Islands</a-select-option>
                  <a-select-option value="TD">TD-乍得 Chad</a-select-option>
                  <a-select-option value="TF">TF-法属南部领地 French Southern Territories</a-select-option>
                  <a-select-option value="TG">TG-多哥 Togo</a-select-option>
                  <a-select-option value="TH">TH-泰国 Thailand</a-select-option>
                  <a-select-option value="TJ">TJ-塔吉克斯坦 Tajikistan</a-select-option>
                  <a-select-option value="TK">TK-托克劳 Tokelau</a-select-option>
                  <a-select-option value="TL">TL-东帝汶 Timor-leste</a-select-option>
                  <a-select-option value="TM">TM-土库曼斯坦 Turkmenistan</a-select-option>
                  <a-select-option value="TN">TN-突尼斯 Tunisia</a-select-option>
                  <a-select-option value="TO">TO-汤加 Tonga</a-select-option>
                  <a-select-option value="TR">TR-土耳其 Turkey</a-select-option>
                  <a-select-option value="TT">TT-特立尼达和多巴哥 Trinidad And Tobago</a-select-option>
                  <a-select-option value="TV">TV-图瓦卢 Tuvalu</a-select-option>
                  <a-select-option value="TW">TW-台湾 Taiwan, Province Of China</a-select-option>
                  <a-select-option value="TZ">TZ-坦桑尼亚 Tanzania, United Republic Of</a-select-option>
                  <a-select-option value="UA">UA-乌克兰 Ukraine</a-select-option>
                  <a-select-option value="UG">UG-乌干达 Uganda</a-select-option>
                  <a-select-option value="UM">UM-美国本土外小岛屿 United States Minor Outlying Islands</a-select-option>
                  <a-select-option value="US">US-美国 United States</a-select-option>
                  <a-select-option value="UY">UY-乌拉圭 Uruguay</a-select-option>
                  <a-select-option value="UZ">UZ-乌兹别克斯坦 Uzbekistan</a-select-option>
                  <a-select-option value="VA">VA-梵蒂冈 Holy See (vatican City State)</a-select-option>
                  <a-select-option value="VC">VC-圣文森特和格林纳丁斯 Saint Vincent And The Grenadines</a-select-option>
                  <a-select-option value="VE">VE-委内瑞拉 Venezuela, Bolivarian Republic Of</a-select-option>
                  <a-select-option value="VG">VG-英属维尔京群岛 Virgin Islands, U.s.</a-select-option>
                  <a-select-option value="VI">VI-美属维尔京群岛 United States Virgin Islands</a-select-option>
                  <a-select-option value="VN">VN-越南 Vietnam</a-select-option>
                  <a-select-option value="VU">VU-瓦努阿图 Vanuatu</a-select-option>
                  <a-select-option value="WF">WF-瓦利斯和富图纳 Wallis And Futuna</a-select-option>
                  <a-select-option value="WS">WS-萨摩亚 Samoa</a-select-option>
                  <a-select-option value="YE">YE-也门 Yemen</a-select-option>
                  <a-select-option value="YT">YT-马约特 Mayotte</a-select-option>
                  <a-select-option value="ZA">ZA-南非 South Africa</a-select-option>
                  <a-select-option value="ZM">ZM-赞比亚 Zambia</a-select-option>
                  <a-select-option value="ZW">ZW-津巴布韦 Zimbabwe</a-select-option>
                </a-select>
              </a-form-model-item>
              <a-form-model-item
                label="邮编"
                :required="true"
                style="margin-bottom:-5px"
              >
                <j-input placeholder="请输入邮编" v-model='formData.postcode'></j-input>
              </a-form-model-item>
            </a-col>


            <a-col :span="12">
              <span>英文地址</span>
              <a-divider />
              <a-form-model-item
                label="联系人"
                :required="true"
                style="margin-bottom:-5px"
              >
                <a-input placeholder="请输入联系人"  v-model='formData.linkman'/>
              </a-form-model-item>
              <a-form-model-item
                label="联系电话"
                :required="true"
                style="margin-bottom:-5px"
              >
                <a-input placeholder="请输入联系电话"  v-model='formData.tel'/>
              </a-form-model-item>
              <a-form-model-item
                label="地址"
                :required="true"
                style="margin-bottom:-5px"
              >
                <a-input placeholder="请输入地址" v-model='formData.addressEn'/>
              </a-form-model-item>
              <a-form-model-item
                label="城市"
                :required="true"
                style="margin-bottom:-5px"
              >
                <j-input placeholder="请输入城市" v-model='this.formData.cityEn' ></j-input>
              </a-form-model-item>
              <a-form-model-item
                label="省/州"
                :required="true"
                style="margin-bottom:-5px"
              >
                <j-input placeholder="请输入省/州" v-model='formData.stateEn'></j-input>
              </a-form-model-item>
              <a-form-model-item
                label="国家"
                :required="true"
                style="margin-bottom:-5px"
              >
                <a-select placeholder="请输入国家" default-value="CN" v-model='formData.country'>
                  <a-select-option value="AD">AD-安道尔 Andorra</a-select-option>
                  <a-select-option value="AE">AE-阿联酋 United Arab Emirates</a-select-option>
                  <a-select-option value="AF">AF-阿富汗 Afghanistan</a-select-option>
                  <a-select-option value="AG">AG-安提瓜和巴布达 Antigua And Barbuda</a-select-option>
                  <a-select-option value="AI">AI-安圭拉 Anguilla</a-select-option>
                  <a-select-option value="AL">AL-阿尔巴尼亚 Albania</a-select-option>
                  <a-select-option value="AM">AM-亚美尼亚 Armenia</a-select-option>
                  <a-select-option value="AO">AO-安哥拉 Angola</a-select-option>
                  <a-select-option value="AQ">AQ-南极洲 Antarctica</a-select-option>
                  <a-select-option value="AR">AR-阿根廷 Argentina</a-select-option>
                  <a-select-option value="AS">AS-美属萨摩亚 American Samoa</a-select-option>
                  <a-select-option value="AT">AT-奥地利 Austria</a-select-option>
                  <a-select-option value="AU">AU-澳大利亚 Australia</a-select-option>
                  <a-select-option value="AW">AW-阿鲁巴 Aruba</a-select-option>
                  <a-select-option value="AX">AX-奥兰群岛 Aland Islands</a-select-option>
                  <a-select-option value="AZ">AZ-阿塞拜疆 Azerbaijan</a-select-option>
                  <a-select-option value="BA">BA-波黑 Bosnia And Herzegovina</a-select-option>
                  <a-select-option value="BB">BB-巴巴多斯 Barbados</a-select-option>
                  <a-select-option value="BD">BD-孟加拉国 Bangladesh</a-select-option>
                  <a-select-option value="BE">BE-比利时 Belgium</a-select-option>
                  <a-select-option value="BF">BF-布基纳法索 Burkina Faso</a-select-option>
                  <a-select-option value="BG">BG-保加利亚 Bulgaria</a-select-option>
                  <a-select-option value="BH">BH-巴林 Bahrain</a-select-option>
                  <a-select-option value="BI">BI-布隆迪 Burundi</a-select-option>
                  <a-select-option value="BJ">BJ-贝宁 Benin</a-select-option>
                  <a-select-option value="BL">BL-圣巴泰勒米岛 Saint Barthélemy</a-select-option>
                  <a-select-option value="BM">BM-百慕大 Bermuda</a-select-option>
                  <a-select-option value="BN">BN-文莱 Brunei Darussalam</a-select-option>
                  <a-select-option value="BO">BO-玻利维亚 Bolivia, Plurinational</a-select-option>
                  <a-select-option value="BQ">BQ-荷兰加勒比区 Bonaire, Sint Eustatius And Saba</a-select-option>
                  <a-select-option value="BR">BR-巴西 Brazil</a-select-option>
                  <a-select-option value="BS">BS-巴哈马 Bahamas</a-select-option>
                  <a-select-option value="BT">BT-不丹 Bhutan</a-select-option>
                  <a-select-option value="BV">BV-布韦岛 Bouvet Island</a-select-option>
                  <a-select-option value="BW">BW-博茨瓦纳 Botswana</a-select-option>
                  <a-select-option value="BY">BY-白俄罗斯 Belarus</a-select-option>
                  <a-select-option value="BZ">BZ-伯利兹 Belize</a-select-option>
                  <a-select-option value="CA">CA-加拿大 Canada</a-select-option>
                  <a-select-option value="CC">CC-科科斯群岛 Cocos (keeling) Islands</a-select-option>
                  <a-select-option value="CD">CD-刚果（金） Congo, The Democratic Republic Of The</a-select-option>
                  <a-select-option value="CF">CF-中非 Central African Republic</a-select-option>
                  <a-select-option value="CG">CG-刚果（布） Congo</a-select-option>
                  <a-select-option value="CH">CH-瑞士 Switzerland</a-select-option>
                  <a-select-option value="CI">CI-科特迪瓦 Côte D&#039;ivoire</a-select-option>
                  <a-select-option value="CK">CK-库克群岛 Cook Islands</a-select-option>
                  <a-select-option value="CL">CL-智利 Chile</a-select-option>
                  <a-select-option value="CM">CM-喀麦隆 Cameroon</a-select-option>
                  <a-select-option value="CN" >CN-中国 People&#039;s Republic Of China</a-select-option>
                  <a-select-option value="CO">CO-哥伦比亚 Colombia</a-select-option>
                  <a-select-option value="CR">CR-哥斯达黎加 Costa Rica</a-select-option>
                  <a-select-option value="CU">CU-古巴 Cuba</a-select-option>
                  <a-select-option value="CV">CV-佛得角 Cape Verde</a-select-option>
                  <a-select-option value="CW">CW-库拉索 Curaçao</a-select-option>
                  <a-select-option value="CX">CX-圣诞岛 Christmas Island</a-select-option>
                  <a-select-option value="CY">CY-塞浦路斯 Cyprus</a-select-option>
                  <a-select-option value="CZ">CZ-捷克 Czech Republic</a-select-option>
                  <a-select-option value="DE">DE-德国 Germany</a-select-option>
                  <a-select-option value="DJ">DJ-吉布提 Djibouti</a-select-option>
                  <a-select-option value="DK">DK-丹麦 Denmark</a-select-option>
                  <a-select-option value="DM">DM-多米尼克 Dominica</a-select-option>
                  <a-select-option value="DO">DO-多米尼加 Dominican Republic</a-select-option>
                  <a-select-option value="DZ">DZ-阿尔及利亚 Algeria</a-select-option>
                  <a-select-option value="EC">EC-厄瓜多尔 Ecuador</a-select-option>
                  <a-select-option value="EE">EE-爱沙尼亚 Estonia</a-select-option>
                  <a-select-option value="EG">EG-埃及 Egypt</a-select-option>
                  <a-select-option value="EH">EH-西撒哈拉 Western Sahara</a-select-option>
                  <a-select-option value="ER">ER-厄立特里亚 Eritrea</a-select-option>
                  <a-select-option value="ES">ES-西班牙 Spain</a-select-option>
                  <a-select-option value="ET">ET-埃塞俄比亚 Ethiopia</a-select-option>
                  <a-select-option value="FI">FI-芬兰 Finland</a-select-option>
                  <a-select-option value="FJ">FJ-斐济群岛 Fiji</a-select-option>
                  <a-select-option value="FK">FK-马尔维纳斯群岛（福克兰） Falkland Islands (malvinas)</a-select-option>
                  <a-select-option value="FM">FM-密克罗尼西亚联邦 Micronesia, Federated States Of</a-select-option>
                  <a-select-option value="FO">FO-法罗群岛 Faroe Islands</a-select-option>
                  <a-select-option value="FR">FR-法国 France</a-select-option>
                  <a-select-option value="GA">GA-加蓬 Gabon</a-select-option>
                  <a-select-option value="GB">GB-英国 United Kingdom</a-select-option>
                  <a-select-option value="GD">GD-格林纳达 Grenada</a-select-option>
                  <a-select-option value="GE">GE-格鲁吉亚 Georgia</a-select-option>
                  <a-select-option value="GF">GF-法属圭亚那 French Guiana</a-select-option>
                  <a-select-option value="GG">GG-根西岛 Guernsey</a-select-option>
                  <a-select-option value="GH">GH-加纳 Ghana</a-select-option>
                  <a-select-option value="GI">GI-直布罗陀 Gibraltar</a-select-option>
                  <a-select-option value="GL">GL-格陵兰 Greenland</a-select-option>
                  <a-select-option value="GM">GM-冈比亚 Gambia</a-select-option>
                  <a-select-option value="GN">GN-几内亚 Guinea</a-select-option>
                  <a-select-option value="GP">GP-瓜德罗普 Guadeloupe</a-select-option>
                  <a-select-option value="GQ">GQ-赤道几内亚 Equatorial Guinea</a-select-option>
                  <a-select-option value="GR">GR-希腊 Greece</a-select-option>
                  <a-select-option value="GS">GS-南乔治亚岛和南桑威奇群岛 South Georgia And The South Sandwich Islands</a-select-option>
                  <a-select-option value="GT">GT-危地马拉 Guatemala</a-select-option>
                  <a-select-option value="GU">GU-关岛 Guam</a-select-option>
                  <a-select-option value="GW">GW-几内亚比绍 Guinea-bissau</a-select-option>
                  <a-select-option value="GY">GY-圭亚那 Guyana</a-select-option>
                  <a-select-option value="HK">HK-香港 Hong Kong</a-select-option>
                  <a-select-option value="HM">HM-赫德岛和麦克唐纳群岛 Heard Island And Mcdonald Islands</a-select-option>
                  <a-select-option value="HN">HN-洪都拉斯 Honduras</a-select-option>
                  <a-select-option value="HR">HR-克罗地亚 Croatia</a-select-option>
                  <a-select-option value="HT">HT-海地 Haiti</a-select-option>
                  <a-select-option value="HU">HU-匈牙利 Hungary</a-select-option>
                  <a-select-option value="ID">ID-印尼 Indonesia</a-select-option>
                  <a-select-option value="IE">IE-爱尔兰 Ireland</a-select-option>
                  <a-select-option value="IL">IL-以色列 Israel</a-select-option>
                  <a-select-option value="IM">IM-马恩岛 Isle Of Man</a-select-option>
                  <a-select-option value="IN">IN-印度 India</a-select-option>
                  <a-select-option value="IO">IO-英属印度洋领地 British Indian Ocean Territory</a-select-option>
                  <a-select-option value="IQ">IQ-伊拉克 Iraq</a-select-option>
                  <a-select-option value="IR">IR-伊朗 Iran, Islamic Republic Of</a-select-option>
                  <a-select-option value="IS">IS-冰岛 Iceland</a-select-option>
                  <a-select-option value="IT">IT-意大利 Italy</a-select-option>
                  <a-select-option value="JE">JE-泽西岛 Jersey</a-select-option>
                  <a-select-option value="JM">JM-牙买加 Jamaica</a-select-option>
                  <a-select-option value="JO">JO-约旦 Jordan</a-select-option>
                  <a-select-option value="JP">JP-日本 Japan</a-select-option>
                  <a-select-option value="KE">KE-肯尼亚 Kenya</a-select-option>
                  <a-select-option value="KG">KG-吉尔吉斯斯坦 Kyrgyzstan</a-select-option>
                  <a-select-option value="KH">KH-柬埔寨 Cambodia</a-select-option>
                  <a-select-option value="KI">KI-基里巴斯 Kiribati</a-select-option>
                  <a-select-option value="KM">KM-科摩罗 Comoros</a-select-option>
                  <a-select-option value="KN">KN-圣基茨和尼维斯 Saint Kitts And Nevis</a-select-option>
                  <a-select-option value="KP">KP-朝鲜 Korea, Democratic People&#039;s Republic Of</a-select-option>
                  <a-select-option value="KR">KR-韩国 Korea, Republic Of</a-select-option>
                  <a-select-option value="KV">KV-科索沃 Kosovo</a-select-option>
                  <a-select-option value="KW">KW-科威特 Kuwait</a-select-option>
                  <a-select-option value="KY">KY-开曼群岛 Cayman Islands</a-select-option>
                  <a-select-option value="KZ">KZ-哈萨克斯坦 Kazakhstan</a-select-option>
                  <a-select-option value="LA">LA-老挝 Lao People&#039;s Democratic Republic</a-select-option>
                  <a-select-option value="LB">LB-黎巴嫩 Lebanon</a-select-option>
                  <a-select-option value="LC">LC-圣卢西亚 Saint Lucia</a-select-option>
                  <a-select-option value="LI">LI-列支敦士登 Liechtenstein</a-select-option>
                  <a-select-option value="LK">LK-斯里兰卡 Sri Lanka</a-select-option>
                  <a-select-option value="LR">LR-利比里亚 Liberia</a-select-option>
                  <a-select-option value="LS">LS-莱索托 Lesotho</a-select-option>
                  <a-select-option value="LT">LT-立陶宛 Lithuania</a-select-option>
                  <a-select-option value="LU">LU-卢森堡 Luxembourg</a-select-option>
                  <a-select-option value="LV">LV-拉脱维亚 Latvia</a-select-option>
                  <a-select-option value="LY">LY-利比亚 Libya</a-select-option>
                  <a-select-option value="MA">MA-摩洛哥 Morocco</a-select-option>
                  <a-select-option value="MC">MC-摩纳哥 Monaco</a-select-option>
                  <a-select-option value="MD">MD-摩尔多瓦 Moldova, Republic Of</a-select-option>
                  <a-select-option value="ME">ME-黑山 Montenegro</a-select-option>
                  <a-select-option value="MF">MF-法属圣马丁 Saint Martin (french Part)</a-select-option>
                  <a-select-option value="MG">MG-马达加斯加 Madagascar</a-select-option>
                  <a-select-option value="MH">MH-马绍尔群岛 Marshall Islands</a-select-option>
                  <a-select-option value="MK">MK-马其顿 Macedonia, The Former Yugoslav Republic Of</a-select-option>
                  <a-select-option value="ML">ML-马里 Mali</a-select-option>
                  <a-select-option value="MM">MM-缅甸 Myanmar</a-select-option>
                  <a-select-option value="MN">MN-蒙古国；蒙古 Mongolia</a-select-option>
                  <a-select-option value="MO">MO-澳门 Macao</a-select-option>
                  <a-select-option value="MP">MP-北马里亚纳群岛 Northern Mariana Islands</a-select-option>
                  <a-select-option value="MQ">MQ-马提尼克 Martinique</a-select-option>
                  <a-select-option value="MR">MR-毛里塔尼亚 Mauritania</a-select-option>
                  <a-select-option value="MS">MS-蒙塞拉特岛 Montserrat</a-select-option>
                  <a-select-option value="MT">MT-马耳他 Malta</a-select-option>
                  <a-select-option value="MU">MU-毛里求斯 Mauritius</a-select-option>
                  <a-select-option value="MV">MV-马尔代夫 Maldives</a-select-option>
                  <a-select-option value="MW">MW-马拉维 Malawi</a-select-option>
                  <a-select-option value="MX">MX-墨西哥 Mexico</a-select-option>
                  <a-select-option value="MY">MY-马来西亚 Malaysia</a-select-option>
                  <a-select-option value="MZ">MZ-莫桑比克 Mozambique</a-select-option>
                  <a-select-option value="NA">NA-纳米比亚 Namibia</a-select-option>
                  <a-select-option value="NC">NC-新喀里多尼亚 New Caledonia</a-select-option>
                  <a-select-option value="NE">NE-尼日尔 Niger</a-select-option>
                  <a-select-option value="NF">NF-诺福克岛 Norfolk Island</a-select-option>
                  <a-select-option value="NG">NG-尼日利亚 Nigeria</a-select-option>
                  <a-select-option value="NI">NI-尼加拉瓜 Nicaragua</a-select-option>
                  <a-select-option value="NK">NK-纳戈尔诺-卡拉巴赫 Nagorno-karabakh</a-select-option>
                  <a-select-option value="NL">NL-荷兰 Netherlands</a-select-option>
                  <a-select-option value="NO">NO-挪威 Norway</a-select-option>
                  <a-select-option value="NP">NP-尼泊尔 Nepal</a-select-option>
                  <a-select-option value="NR">NR-瑙鲁 Nauru</a-select-option>
                  <a-select-option value="NU">NU-纽埃 Niue</a-select-option>
                  <a-select-option value="NZ">NZ-新西兰 New Zealand</a-select-option>
                  <a-select-option value="OM">OM-阿曼 Oman</a-select-option>
                  <a-select-option value="PA">PA-巴拿马 Panama</a-select-option>
                  <a-select-option value="PE">PE-秘鲁 Peru</a-select-option>
                  <a-select-option value="PF">PF-法属波利尼西亚 French Polynesia</a-select-option>
                  <a-select-option value="PG">PG-巴布亚新几内亚 Papua New Guinea</a-select-option>
                  <a-select-option value="PH">PH-菲律宾 Philippines</a-select-option>
                  <a-select-option value="PK">PK-巴基斯坦 Pakistan</a-select-option>
                  <a-select-option value="PL">PL-波兰 Poland</a-select-option>
                  <a-select-option value="PM">PM-圣皮埃尔和密克隆 Saint Pierre And Miquelon</a-select-option>
                  <a-select-option value="PN">PN-皮特凯恩群岛 Pitcairn Islands</a-select-option>
                  <a-select-option value="PR">PR-波多黎各 Puerto Rico</a-select-option>
                  <a-select-option value="PS">PS-巴勒斯坦 Palestine, State Of</a-select-option>
                  <a-select-option value="PT">PT-葡萄牙 Portugal</a-select-option>
                  <a-select-option value="PW">PW-帕劳 Palau</a-select-option>
                  <a-select-option value="PY">PY-巴拉圭 Paraguay</a-select-option>
                  <a-select-option value="QA">QA-卡塔尔 Qatar</a-select-option>
                  <a-select-option value="RE">RE-留尼汪 Réunion</a-select-option>
                  <a-select-option value="RO">RO-罗马尼亚 Romania</a-select-option>
                  <a-select-option value="RS">RS-塞尔维亚 Serbia</a-select-option>
                  <a-select-option value="RU">RU-俄罗斯 Russian Federation</a-select-option>
                  <a-select-option value="RW">RW-卢旺达 Rwanda</a-select-option>
                  <a-select-option value="SA">SA-沙特阿拉伯 Saudi Arabia</a-select-option>
                  <a-select-option value="SB">SB-所罗门群岛 Solomon Islands</a-select-option>
                  <a-select-option value="SC">SC-塞舌尔 Seychelles</a-select-option>
                  <a-select-option value="SD">SD-苏丹 Sudan</a-select-option>
                  <a-select-option value="SE">SE-瑞典 Sweden</a-select-option>
                  <a-select-option value="SG">SG-新加坡 Singapore</a-select-option>
                  <a-select-option value="SH">SH-圣赫勒拿 Saint Helena, Ascension And Tristan Da Cunha</a-select-option>
                  <a-select-option value="SI">SI-斯洛文尼亚 Slovenia</a-select-option>
                  <a-select-option value="SJ">SJ-斯瓦尔巴群岛和扬马延岛 Svalbard And Jan Mayen</a-select-option>
                  <a-select-option value="SK">SK-斯洛伐克 Slovakia</a-select-option>
                  <a-select-option value="SL">SL-塞拉利昂 Sierra Leone</a-select-option>
                  <a-select-option value="SM">SM-圣马力诺 San Marino</a-select-option>
                  <a-select-option value="SN">SN-塞内加尔 Senegal</a-select-option>
                  <a-select-option value="SO">SO-索马里 Somalia</a-select-option>
                  <a-select-option value="SR">SR-苏里南 Suriname</a-select-option>
                  <a-select-option value="SS">SS-南苏丹 South Sudan</a-select-option>
                  <a-select-option value="ST">ST-圣多美和普林西比 Sao Tome And Principe</a-select-option>
                  <a-select-option value="SV">SV-萨尔瓦多 El Salvador</a-select-option>
                  <a-select-option value="SX">SX-荷属圣马丁 Sint Maarten (dutch Part)</a-select-option>
                  <a-select-option value="SY">SY-叙利亚 Syrian Arab Republic</a-select-option>
                  <a-select-option value="SZ">SZ-斯威士兰 Swaziland</a-select-option>
                  <a-select-option value="TC">TC-特克斯和凯科斯群岛 Turks And Caicos Islands</a-select-option>
                  <a-select-option value="TD">TD-乍得 Chad</a-select-option>
                  <a-select-option value="TF">TF-法属南部领地 French Southern Territories</a-select-option>
                  <a-select-option value="TG">TG-多哥 Togo</a-select-option>
                  <a-select-option value="TH">TH-泰国 Thailand</a-select-option>
                  <a-select-option value="TJ">TJ-塔吉克斯坦 Tajikistan</a-select-option>
                  <a-select-option value="TK">TK-托克劳 Tokelau</a-select-option>
                  <a-select-option value="TL">TL-东帝汶 Timor-leste</a-select-option>
                  <a-select-option value="TM">TM-土库曼斯坦 Turkmenistan</a-select-option>
                  <a-select-option value="TN">TN-突尼斯 Tunisia</a-select-option>
                  <a-select-option value="TO">TO-汤加 Tonga</a-select-option>
                  <a-select-option value="TR">TR-土耳其 Turkey</a-select-option>
                  <a-select-option value="TT">TT-特立尼达和多巴哥 Trinidad And Tobago</a-select-option>
                  <a-select-option value="TV">TV-图瓦卢 Tuvalu</a-select-option>
                  <a-select-option value="TW">TW-台湾 Taiwan, Province Of China</a-select-option>
                  <a-select-option value="TZ">TZ-坦桑尼亚 Tanzania, United Republic Of</a-select-option>
                  <a-select-option value="UA">UA-乌克兰 Ukraine</a-select-option>
                  <a-select-option value="UG">UG-乌干达 Uganda</a-select-option>
                  <a-select-option value="UM">UM-美国本土外小岛屿 United States Minor Outlying Islands</a-select-option>
                  <a-select-option value="US">US-美国 United States</a-select-option>
                  <a-select-option value="UY">UY-乌拉圭 Uruguay</a-select-option>
                  <a-select-option value="UZ">UZ-乌兹别克斯坦 Uzbekistan</a-select-option>
                  <a-select-option value="VA">VA-梵蒂冈 Holy See (vatican City State)</a-select-option>
                  <a-select-option value="VC">VC-圣文森特和格林纳丁斯 Saint Vincent And The Grenadines</a-select-option>
                  <a-select-option value="VE">VE-委内瑞拉 Venezuela, Bolivarian Republic Of</a-select-option>
                  <a-select-option value="VG">VG-英属维尔京群岛 Virgin Islands, U.s.</a-select-option>
                  <a-select-option value="VI">VI-美属维尔京群岛 United States Virgin Islands</a-select-option>
                  <a-select-option value="VN">VN-越南 Vietnam</a-select-option>
                  <a-select-option value="VU">VU-瓦努阿图 Vanuatu</a-select-option>
                  <a-select-option value="WF">WF-瓦利斯和富图纳 Wallis And Futuna</a-select-option>
                  <a-select-option value="WS">WS-萨摩亚 Samoa</a-select-option>
                  <a-select-option value="YE">YE-也门 Yemen</a-select-option>
                  <a-select-option value="YT">YT-马约特 Mayotte</a-select-option>
                  <a-select-option value="ZA">ZA-南非 South Africa</a-select-option>
                  <a-select-option value="ZM">ZM-赞比亚 Zambia</a-select-option>
                  <a-select-option value="ZW">ZW-津巴布韦 Zimbabwe</a-select-option>
                </a-select>
              </a-form-model-item>
              <a-form-model-item
                label="邮编"
                :required="true"
              >
                <j-input placeholder="请输入邮编" v-model='formData.postcode'></j-input>
              </a-form-model-item>
            </a-col>
          </a-row>

          <a-form-model-item>
            <a-button type="primary" style='margin-right: 5px '  @click="edit" v-if="formData.status=='1'">保存修改</a-button>
            <a-button type="primary"  style='margin-right: 5px ' @click="pass" v-if="formData.status=='1'||formData.status=='3'">通过</a-button>
            <a-button type="danger"  @click="edit" v-if='formData.status==1'>拒绝</a-button>
            <a-button type="danger"  @click="passOut" v-if='formData.status==4'>审核驳回</a-button>
            <!--            <a-button style="margin-left: 8px">保存</a-button>-->
          </a-form-model-item>
        </a-form-model>

      </a-col>
      <!--      <a-col :md="24" :lg="8" :style="{ minHeight: '180px' }">-->
      <!--        <div class="ant-upload-preview" @click="$refs.modal.edit(1)" >-->
      <!--          <a-icon type="cloud-upload-o" class="upload-icon"/>-->
      <!--          <div class="mask">-->
      <!--            <a-icon type="plus" />-->
      <!--          </div>-->
      <!--          <img :src="option.img"/>-->
      <!--        </div>-->
      <!--      </a-col>-->

    </a-row>

    <avatar-modal ref="modal">

    </avatar-modal>
  </div>
</template>

<script>
import AvatarModal from '../user/settings/AvatarModal'
import { mapGetters } from 'vuex'
import { pinyin } from 'pinyin-pro'
import { handleDetailss,putAction,postAction  } from '../../../api/manage'
import JInput from '@comp/jeecg/JInput'
import JUpload from '@/components/jeecg/JUpload'
import JImageUpload from '@/components/jeecg/JImageUpload'
export default {
  name: 'ZmClientIdentity',
  components: {
    AvatarModal,
    JInput,
    JImageUpload,
    JUpload,

  },
  props:{
    mainId:{
      type:String,
      default:'',
      required:false
    }
  },
  watch:{
    mainId:{
      immediate: true,
      handler(val) {
        this.formData={},
        this.getDetails();
      }
    }
  },
  data () {
    return {
      // cropper

      preview: {},
      imgList:[],
      option: {
        img: '/avatar2.jpg',
        info: true,
        size: 1,
        outputType: 'jpeg',
        canScale: false,
        autoCrop: true,
        // 只有自动截图开启 宽度高度才生效
        autoCropWidth: 180,
        autoCropHeight: 180,
        fixedBox: true,
        // 开启宽度和高度比例
        fixed: true,
        fixedNumber: [1, 1],
        status:0,
      },
      formData:{
        type:'',
        company:'',
        license:'',
        linkman:'',
        tel:'',
        appendix:'',
        address:'',
        city:'',
        state:'',
        country:'',
        postcode:'',
        addressEn:'',
        cityEn:'',
        stateEn:'',
        status:'',
      }
    }
  },
  created() {
      this.getDetails();
  },
  methods: {
    ...mapGetters(["nickname", "avatar","userInfo","userIdentity"]),
    handleSubmit(e) {

      e.preventDefault();
      this.form.validateFields((err, values) => {
        if (!err) {
          console.log('Received values of form: ', values);
        }
      });
    },
    onSubmit() {
      this.$refs.ruleForm.validate(valid => {
        if (valid) {
          postAction("/zmexpress/zmUserDetail/add", this.formData).then((res) => {
            if(res.success){
              if(res.code == 201){
                this.errorTip(res.message, res.result)
              }else{
                this.$message.success(res.message)
              }
              this.$emit('ok')
            }else{
              this.$message.warning(res.message)
            }
          })
        } else {
          console.log('error submit!!');
          return false;
        }
      });
    },

    pass() {
      this.$refs.ruleForm.validate(valid => {
        if (valid) {
          putAction("/sys/user/pass", this.formData).then((res) => {
            if(res.success){
              if(res.code == 201){
                this.errorTip(res.message, res.result)
              }else{
                this.getDetails()
                this.$message.success(res.message)
              }
              this.$emit('ok')
              this.$emit("sortNum")
            }else{

              this.$message.warning(res.message)
            }
          })
        } else {
          return false;
        }
      });
    },
    passOut() {
      this.$refs.ruleForm.validate(valid => {
        if (valid) {
          putAction("/sys/user/passOut", this.formData).then((res) => {
            if(res.success){
              if(res.code == 201){
                this.errorTip(res.message, res.result)
              }else{
                this.getDetails()
                this.$message.success(res.message)
              }
              this.$emit('ok')
              this.$emit("sortNum")
            }else{
              this.$message.warning(res.message)
            }
          })
        } else {
          return false;
        }
      });
    },
    edit() {
      this.$refs.ruleForm.validate(valid => {
        if (valid) {
          putAction("/zmexpress/zmUserDetail/edit", this.formData).then((res) => {
            if(res.success){
              if(res.code == 201){
                this.errorTip(res.message, res.result)
              }else{
                this.$message.success(res.message)
              }
              this.$emit('ok')
            }else{
              this.$message.warning(res.message)
            }
          })
        } else {
          return false;
        }
      });
    },
    toPingying(val){
      if (val=='address'){
        var str=pinyin(this.formData.address,{ toneType: 'none' });
        this.formData.addressEn = str.toUpperCase();
      }else if (val=='city'){
        var str=pinyin(this.formData.city,{ toneType: 'none' });
        this.formData.cityEn = str.toUpperCase();
      }else if (val=='state'){
        var str=pinyin(this.formData.state,{ toneType: 'none' });
        this.formData.stateEn = str.toUpperCase();
      }

    },
    resetForm() {
      this.$refs.ruleForm.resetFields();
    },
    async getDetails(){
      let data;
      data =await handleDetailss( '/zmexpress/zmUserDetail/queryByUserCode', {userCode: this.mainId}).then(res => {
        if (res.success) {
          console.log("res",res.result)
          return res.result;
        }
      });

      if (data!=null){
        this.formData = data;
        this.status = data.status;
        console.log("formData",data);
        console.log("status",this.status);
      }


    },
    // 校验手机号
    validateMobile(rule,value,callback){
      if (!value || new RegExp(/^1([38][0-9]|4[579]|5[0-3,5-9]|6[6]|7[0135678]|9[89])\d{8}$/).test(value)){
        callback();
      }else{
        callback("您的手机号码格式不正确!");
      }
    },
  }
}
</script>

<style scoped>

</style>