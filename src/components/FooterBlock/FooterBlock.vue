<template>
  <section
    id="contacts"
    class="footer-block"
  >
    <v-container class="Container mxw1200">
      <v-row>
        <v-col
          cols="12"
          sm="6"
          lg="3"
          class="marwieght"
        >
          <h4>{{ $t('footerBlock.head1') }}</h4>
          <div class="directions">
            <a
              v-for="(direction, i) in $t('footerBlock.directions')"
              :key="`direction${i}`"
              :href="`${direction.link}`"
              target="_blank"
              @click="`${SubmitBTN(direction.click)}`"
            ><img
              :src="`/img/directions/${direction.img}.svg`"
            ></a>
          </div>
        </v-col>
        <v-col
          cols="12"
          sm="6"
          lg="3"
        >
          <h4>{{ $t('footerBlock.head2') }}</h4>
          <div class="menu">
            <a
              v-for="(menuItem, i) in footerItems"
              :key="`menuItem${i}`"
              :href="`${menuItem.link}`"
              @click="`${SubmitBTN(menuItem.click)}`"
              target="`${menuItem.target}`"
            >
              {{ menuItem.text }}
            </a>
          </div>
        </v-col>
        <v-col
          cols="12"
          sm="6"
          lg="3"
        >
          <h4>{{ $t('footerBlock.head3') }}</h4>
          <div class="address">
            <p>
              <b style="font-weight: 500"><a
                :href="`tel:${addressItems.numberLink}`"
                @click="`${Submit_tel2()}`"
              >{{ addressItems.number }}</a></b>
            </p>
            <p>
              <b style="font-weight: 500"><a
                :href="`mailto:${addressItems.email}`"
                @click="`${Submit_EmailLetter()}`"
              >{{ addressItems.email }}</a></b>
            </p>
            <p class="op07">
              {{ addressItems.city }}
            </p>
            <p class="op07">
              {{ addressItems.address }}
            </p>
            <p class="op07">
              {{ addressItems.support }}
            </p>
            <p>
              <b style="font-weight: 500"><a
                :href="`mailto:${addressItems.support_email}`"
                @click="`${Submit_EmailLetter()}`"
              >{{ addressItems.support_email }}</a></b>
            </p>
          </div>
        </v-col>
        <v-col
          cols="12"
          sm="6"
          lg="3"
        >
          <h4>{{ $t('footerBlock.head4') }}</h4>
          <div class="subscription">
            <v-text-field
              dark
              v-model="email"
              :label="$t('footerBlock.email')"
              required
            />
            <v-checkbox
              dark
              v-model="agreeCheck"
              class="CheckBoxUp"
            >
              <template #label>
                <span
                  @click.stop
                  class="agreeCheckLabel"
                  v-html="$t('footerBlock.agreeCheckLabel')"
                />
              </template>
            </v-checkbox>
            <v-btn
              
              href="#targetSubscription"
              color="primaryColor"
              dark
              @click="formSubSend($t('lang'))"
              class="footerBtn"
            >
              {{ $t('footerBlock.subscribeBtn') }}
            </v-btn>
          </div>
        </v-col>
      </v-row>
      <v-row class="bottomFooterBlock">
        <v-col
          cols="12"
          sm="6"
          lg="3"
        >
          <div class="directions mweight">
            <a
              href="/"
            ><img
              src="/img/directions/Logo_ACRYL_Platform.svg"
            ></a>
          </div>
        </v-col>
        <v-col
          cols="12"
          sm="6"
          lg="3"
        >
          <div class="polit">
            <a href="/privacy">
              Privacy Policy
            </a>
            <a href="/cookie">
              Cookie Policy
            </a>
          </div>
        </v-col>
        <v-col
          cols="12"
          sm="6"
          lg="3"
          class="iconwieght"
        >
          <div class="social">
            <a
              v-for="(item, i) in $t('footerBlock.iconItems')"
              :key="`iconSocial${i}`"
              :href="`${item.link}`"
              target="_blank"
              rel="noreferrer noopener"
              @click="`${SubmitBTN(item.click)}`"
            >
              <img
                :src="`/img/social/icon_social_${item.icon}.svg`"
                :alt="`${item.icon}`"
              >
            </a>
          </div>
        </v-col>
        <v-col
          cols="12"
          sm="6"
          lg="3"
        >
          <div class="copyright">
            <p>{{ $t('footerBlock.rule') }}</p>
            <div class="tooltip">
              <v-tooltip
                top
                max-width="300"
                color="black"
              >
                <template #activator="{ on }">
                  <span
                    class="tooltipSpan"
                    style="color: #2EA9FB;"
                    v-on="on"
                  >{{ $t('footerBlock.rules') }}</span>
                </template>
                <span>
                  {{ $t('footerBlock.ruleworld') }}
                </span>
              </v-tooltip>
            </div>
          </div>
        </v-col>
      </v-row>
    </v-container>

    <div
      class="modalOpen"
      v-if="modalTrue"
    >
      <div class="modalCards">
        <v-card
          class="modalCard"
          max-width="344"
          max-height="344"
          outlined
        >
          <p>{{ textError }}</p>
          <v-btn
            rounded
            outlined
            @click="reversModal()"
          >
            Close
          </v-btn>
        </v-card>
      </div>
    </div>
  </section>
</template>

<script>
import axios from "axios";

export default {
  name: "FooterBlock",
  props: {
    footerItems: {
      type: Array,
      default: null
    },
    addressItems: {
      type: Object,
      default: null
    }
  },
  computed: {
    getFooterItems() {
      return this.footerItems;
    }
  },
  data() {
    return {
      email: "",
      agreeCheck: false,
      modalTrue: false,
      textError: ""
    };
  },
  methods: {
    formSubSend: function(lang) {
      let formData = new FormData();
      formData.append("emailAddress", this.email);
      const url = (lang === 'en') ?
        "https://docs.google.com/forms/u/0/d/e/1FAIpQLSd0WYX5UExZfAkVjy_UYzl70mJClC-NZqgDcPeQlB--n-PXIQ/formResponse" :
        "https://docs.google.com/forms/u/0/d/e/1FAIpQLSd1iMTg152R7Ev_Gh-P2PiLrhPZL0QMGnqOKDplMbTc9SpJlw/formResponse";

      if (this.agreeCheck) {
        axios
          .post(url,
            formData,
            {}
          )
          .then(response => {
            console.log("response", response);
            this.email = "";
            this.modalTrue = true;
            this.textError = "Form Submitted Successfully";
          })
          .catch(error => {
            console.log(error);
            this.email = "";
            this.modalTrue = true;
            this.textError = "Form Submitted Successfully";
          });
      } else {
        this.modalTrue = true;
        this.textError = "You did not agree with the processing of personal data";
      }
      this.Submit_EmailSubscription();
    },
    reversModal: function() {
      this.modalTrue = !this.modalTrue;
      this.textError = "";
    },
    SubmitBTN(target) {
      switch (target) {
        case "Submit_EmailSubscription":
          this.Submit_EmailSubscription();
          break;
        case "Submit_FB":
          this.Submit_FB();
          break;
        case "Submit_TG":
          this.Submit_TG();
          break;
        case "Submit_TW":
          this.Submit_TW();
          break;
        case "Submit_IG":
          this.Submit_IG();
          break;
        case "Submit_VK":
          this.Submit_VK();
          break;
        case "Submit_github":
          this.Submit_github();
          break;
        case "Submit_EmailLetter":
          this.Submit_EmailLetter();
          break;
        case "Submit_tel2":
          this.Submit_tel2();
          break;
        case "Click_order4":
          this.Click_order4();
          break;
        case "Click_contact":
          this.Click_contact();
          break;
        case "Submit_Blog2":
          this.Submit_Blog2();
          break;
        case "Submit_support":
          this.Submit_support();
          break;
        case "Click_more":
          this.Click_more();
          break;
        case "Submit_platform":
          this.Submit_platform();
          break;
        case "Submit_1c":
          this.Submit_1c();
          break;
        case "Submit_CDN":
          this.Submit_CDN();
          break;
        case "Submit_Enterprise":
          this.Submit_Enterprise();
          break;
        default:
          break;
      }
    },
    Submit_EmailSubscription() {
      console.log("Submit_EmailSubscription");
      window.gaSendButton("Submit_EmailSubscription");
      window.yaSendButton("Submit_EmailSubscription");
    },
    Submit_FB() {
      console.log("Submit_FB");
      window.gaSendButton("Submit_FB");
      window.yaSendButton("Submit_FB");
    },
    Submit_TG() {
      console.log("Submit_TG");
      window.gaSendButton("Submit_TG");
      window.yaSendButton("Submit_TG");
    },
    Submit_TW() {
      console.log("Submit_TW");
      window.gaSendButton("Submit_TW");
      window.yaSendButton("Submit_TW");
    },
    Submit_VK() {
      console.log("Submit_TW");
      window.gaSendButton("Submit_TW");
      window.yaSendButton("Submit_TW");
    },
    Submit_IG() {
      console.log("Submit_IG");
      window.gaSendButton("Submit_IG");
      window.yaSendButton("Submit_IG");
    },
    Submit_github() {
      console.log("Submit_github");
      window.gaSendButton("Submit_github");
      window.yaSendButton("Submit_github");
    },
    Submit_EmailLetter() {
      console.log("Submit_EmailLetter");
      window.gaSendButton("Submit_EmailLetter");
      window.yaSendButton("Submit_EmailLetter");
    },
    Submit_tel2() {
      console.log("Submit_tel2");
      window.gaSendButton("Submit_tel2");
      window.yaSendButton("Submit_tel2");
    },
    Click_order4() {
      console.log("Click_order4");
      window.gaSendButton("Click_order4");
      window.yaSendButton("Click_order4");
    },
    Click_contact() {
      console.log("Click_contact");
      window.gaSendButton("Click_contact");
      window.yaSendButton("Click_contact");
    },
    Submit_Blog2() {
      console.log("Submit_Blog2");
      window.gaSendButton("Submit_Blog2");
      window.yaSendButton("Submit_Blog2");
    },
    Submit_support() {
      console.log("Submit_support");
      window.gaSendButton("Submit_support");
      window.yaSendButton("Submit_support");
    },
    Click_more() {
      console.log("Click_more");
      window.gaSendButton("Click_more");
      window.yaSendButton("Click_more");
    },
    Submit_platform() {
      console.log("Submit_platform");
      window.gaSendButton("Submit_platform");
      window.yaSendButton("Submit_platform");
    },
    Submit_1c() {
      console.log("Submit_1c");
      window.gaSendButton("Submit_1c");
      window.yaSendButton("Submit_1c");
    },
    Submit_CDN() {
      console.log("Submit_CDN");
      window.gaSendButton("Submit_CDN");
      window.yaSendButton("Submit_CDN");
    },
    Submit_Enterprise() {
      console.log("Submit_Enterprise");
      window.gaSendButton("Submit_Enterprise");
      window.yaSendButton("Submit_Enterprise");
    }
  }
};
</script>

<style lang="scss" scoped>
@import "../../assets/styles/index";
.footer-block {
  color: white;
  background-color: $secondaryColor;
  h4 {
    opacity: 0.7;
    font-size: 16px;
    line-height: 20px;
    font-weight: 300;
  }
  .directions {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    margin: 20px 0;
    img {
      height: 20px;
      margin: 10px 0;
    }
  }
  .menu {
    display: flex;
    flex-direction: column;
    margin: 20px 0;
    font-weight: 500;
    a {
      color: white;
      text-decoration: none;
      font-weight: 500;
      font-size: 16px;
      line-height: 20px;
      padding-bottom: 10px;
      &:hover {
        text-decoration: underline;
      }
    }
  }
  .address {
    margin: 20px 0;
    p {
      padding: 0;
      margin: 0;
      &.op07 {
        opacity: 0.7;
      }
    }
    a {
      color: white;
      text-decoration: none;
      &:hover {
        text-decoration: underline
      }
    }
  }
  .subscription {
    margin: 20px 0;
    max-width: 300px;
    display: flex;
    justify-content: center;
    align-items: flex-start;
    flex-direction: column;
    @include respond-to(large-screens) {
     justify-content: flex-start;
    }
    .v-input {
      margin: 0;
      padding: 0;
    }
    .fieldSub {
      color: white;
    }
    .footerBtn{
      border: 1px solid white;
    }
  }
  .polit {
    display: flex;
    align-items: center;
    height: 100%;
    a {
      color: white;
      text-decoration: none;
      margin: 0 10px 0 0;
      font-weight: 500;
      font-size: 16px;
      line-height: 20px;
      &:hover {
        text-decoration: underline;
      }
    }
  }
  .social {
    display: flex;
    align-items: center;
    height: 100%;
    a {
      margin: 0 20px 0 0;
    }
  }
  .copyright {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-items: center;
    justify-content: center;
    height: 100%;
    font-style: normal;
    font-weight: normal;
    font-size: 12px;
    p {
      margin: 0;
      padding: 0;
    }
    a {
      margin: 0 20px 0 0;
    }
  }
}
.agreeCheckLabel {
  font-style: normal;
  font-weight: normal;
  font-size: 11px;
  line-height: 12px;
}
.modalOpen {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.7);
  z-index: 1000;
  .modalCards {
    position: relative;
    width: 100%;
    height: 100vh;
    .modalCard {
      margin: 25% auto;
      display: flex;
      flex-direction: column;
      align-content: center;
      align-items: center;
      justify-content: center;
      padding: 20px;
      p {
        text-align: center;
      }
    }
  }
}
</style>
