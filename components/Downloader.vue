<template>
  <div class="live-downloader py-5 mb-5 sm:mb-10 px-3">
    <form class="sm:flex justify-center">
      <!-- Select: OS -->
      <div class="select-wrapper md:mr-10 mb-4 sm:mb-0">
        <label for="inputOS" class="mr-3 mb-2 sm:mb-0 text-uppercase font-bold"
          >系统</label
        >
        <select id="inputOS" class="form-control" v-model="os">
          <option selected>选择...</option>
          <option value="mac_intel">macOS (Intel)</option>
          <option value="mac_silicon">macOS (Silicon)</option>
          <option value="win">Windows</option>
        </select>
      </div>

      <!-- Select: Version -->
      <div class="select-wrapper md:mr-10 mb-4 sm:mb-0">
        <label
          for="inputVersion"
          class="mr-3 mb-2 sm:mb-0 text-uppercase font-bold"
          >版本</label
        >
        <select id="inputVersion" class="form-control" v-model="version">
          <option selected>选择...</option>
          <option v-for="version in versions" :key="version.id">
            {{ version }}
          </option>
        </select>
      </div>

      <!-- Select: Edition -->
      <div class="select-wrapper md:mr-10 mb-5 sm:mb-0">
        <label
          for="inputEdition"
          class="mr-3 mb-2 sm:mb-0 text-uppercase font-bold"
          >版本</label
        >
        <select id="inputEdition" class="form-control" v-model="edition">
          <option selected>选择...</option>
          <option value="suite">Suite</option>
          <option value="standard">Standard</option>
          <option value="intro">Intro</option>
          <option value="trial">Trial</option>
        </select>
      </div>

      <button
        type="submit"
        class="btn bg-white text-black hover:opacity-75 transition-color duration-200 uppercase px-3 py-3 sm:py-0 font-bold mt-3 md:mt-0"
        @click.prevent="download"
      >
        下载
      </button>
    </form>
  </div>
</template>

<script>
import versions from "../data/versions.json";
import { compareVersions } from "compare-versions";

export default {
  data() {
    return {
      // Example path:
      // https://cdn-downloads.ableton.com/channels/10.0.6/ableton_live_suite_10.0.6_64.dmg

      cdn_path: "https://cdn-downloads.ableton.com/channels/",
      versions: versions,
      os: "选择...",
      edition: "选择...",
      version: "选择...",
    };
  },

  methods: {
    download: function () {
      let url = this.cdn_path;

      if (
        this.os == "选择..." ||
        this.edition == "选择..." ||
        this.version == "选择..."
      ) {
        alert("请在下拉菜单里选择一个版本");
      } else {
        url += this.version + "/ableton_live_suite_" + this.version;

        if (this.os.startsWith("mac")) {
          // Updates for Apple Silicon universal binary
          if (compareVersions(this.version, "11.0.12") > 0) {
            if (this.os == "mac_intel") {
              url += "_intel.dmg";
            } else {
              url += "_universal.dmg";
            }
          } else {
            url += "_64.dmg";
          }
        } else {
          url += "_64.zip";
        }

        window.open(url);
      }
    },
  },
};
</script>

<style>
.select-wrapper {
  position: relative;
  display: flex;
  align-items: center;
}

.select-wrapper::after {
  content: "";
  position: absolute;
  right: 10px;
  width: 12px;
  height: 8px;
  border-style: solid;
  border-width: 8px 6px 0 6px;
  border-color: #ffffff transparent transparent transparent;
  pointer-events: none;
}

select {
  padding: 5px;
}

.form-control,
.form-control:focus {
  outline: 0;
  border: 2px solid white;
  background: black;
  color: white;
  border-radius: 0;
  font-weight: bold;
  -webkit-appearance: none;
  text-align: left;
  min-width: 130px;
}

@media screen and (max-width: 575px) {
  .select-wrapper {
    flex-direction: column;
  }
  .select-wrapper:after {
    bottom: 17px;
  }
  .btn,
  .form-control {
    width: 100%;
  }
  form .select-wrapper {
    width: 100%;
  }
}

@media screen and (max-width: 768px) {
  .form-inline {
    display: block;
  }
  .form-group:after {
    right: 13px;
  }
  .select-wrapper {
    /* background: red; */
    width: 280px;
  }
  .select-wrapper label {
    width: 100px;
  }
  .form-control {
    min-width: 160px;
  }
}
</style>
