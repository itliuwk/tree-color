<template>
  <div>
    <vue2-org-tree
      :data="data"
      :horizontal="true"
      name="test"
      :NodeClass="NodeClass"
      :judge="judge"
      :label-class-name="labelClassName"
      collapsable
      :renderContent="renderContent"
      @on-expand="onExpand"
      @on-node-mouseover="onMouseover"
      @on-node-mouseout="onMouseout"
    />
    <div v-show="BasicSwich" class="floating">
      <p>ID:{{ BasicInfo.id }}</p>
      <p>Name:{{ BasicInfo.label }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      BasicSwich: false,
      BasicInfo: { id: null, label: null },
      judge: { swtich: true },
      NodeClass: ["myred", "myger", "myblue"],
      data: {
        id: 0,
        label: "机柜",
        imgSrc:
          "http://images.sxitw.cn/%E4%B8%9A%E5%8A%A1%E6%9C%8D%E5%8A%A1.png",
        children: [
          {
            id: 2,
            label: "系统平台层",
            children: [
              {
                id: 5,
                label: "中间件实例",
                swtich: "myred",
                imgSrc:
                  "http://images.sxitw.cn/%E4%B8%AD%E9%97%B4%E4%BB%B6%E5%AE%9E%E4%BE%8B.png",
              },
              {
                id: 6,
                label: "以太网交换机",
                swtich: "myger",
                imgSrc:
                  "http://images.sxitw.cn/%E5%BA%94%E7%94%A8%E5%AE%9E%E4%BE%8B.png",
              },
              {
                id: 9,
                label: "光纤交换机",
                swtich: 111,
                imgSrc:
                  "http://images.sxitw.cn/%E5%BA%94%E7%94%A8%E5%AE%9E%E4%BE%8B.png",
              },
            ],
          },
          {
            id: 3,
            label: "应用层",
            imgSrc: "",
            children: [
              {
                id: 7,
                label: "应用实例",
                swtich: "myblue",
                imgSrc:
                  "http://images.sxitw.cn/%E6%95%B0%E6%8D%AE%E5%BA%93%E5%AE%9E%E4%BE%8B.png",
              },
              {
                id: 8,
                label: "数据库实例",
                swtich: false,
                imgSrc:
                  "http://images.sxitw.cn/%E6%95%B0%E6%8D%AE%E5%BA%93%E5%AE%9E%E4%BE%8B.png",
              },
            ],
          },
          {
            id: 4,
            label: "业务层",
            imgSrc: "",
          },
          {
            id: 9,
            label: "网络层",
            imgSrc: "",
          },
        ],
      },
      horizontal: false,
      collapsable: true,
      expandAll: false,
      labelClassName: "org-bg-res",
    };
  },
  created() {
    this.expandChange();
  },
  methods: {
    renderContent(h, data) {
      return (
        <div>
          {data.imgSrc && (
            <img style="width:40px;height:40px" src={data.imgSrc} />
          )}
          <div>{data.label}</div>
        </div>
      );
    },
    onMouseout(e, data) {
      this.BasicSwich = false;
    },
    onMouseover(e, data) {
      this.BasicInfo = data;
      this.BasicSwich = true;
      var floating = document.getElementsByClassName("floating")[0];
      floating.style.left = e.clientX + 10 + "px";
      floating.style.top = e.clientY + 10 + "px";
    },
    onExpand(e, data) {
      if ("expand" in data) {
        data.expand = !data.expand;
        if (!data.expand && data.children) {
          this.collapse(data.children);
        }
      } else {
        this.$set(data, "expand", true);
      }
    },
    NodeClick(e, data) {
      alert(data.label);
      console.log(e, data);
    },
    collapse(list) {
      var _this = this;
      list.forEach(function (child) {
        if (child.expand) {
          child.expand = false;
        }
        child.children && _this.collapse(child.children);
      });
    },
    expandChange() {
      this.toggleExpand(this.data, true);
    },
    toggleExpand(data, val) {
      var _this = this;
      if (Array.isArray(data)) {
        data.forEach(function (item) {
          _this.$set(item, "expand", val);
          if (item.children) {
            _this.toggleExpand(item.children, val);
          }
        });
      } else {
        this.$set(data, "expand", val);
        if (data.children) {
          _this.toggleExpand(data.children, val);
        }
      }
    },
  },
};
</script>
<style lang="less" >
.myblue {
  background: skyblue;
  color: #fff;
}
.myred {
  background-color: tomato;
  color: #fff;
}
.myger {
  background: green;
  color: #fff;
}
.org-bg-err {
  background-color: tomato;
  color: #fff;
}
.org-tree-node-label {
  white-space: nowrap;
}
.bg-white {
  background-color: white;
}
.org-bg-res {
  background-color: orange;
  color: #fff;
  cursor: pointer;
}
.bg-gold {
  background-color: gold;
}
.bg-gray {
  background-color: gray;
}
.bg-lightpink {
  background-color: lightpink;
}
.bg-chocolate {
  background-color: chocolate;
}
.bg-tomato {
  background-color: tomato;
}
.floating {
  background: rgba(0, 0, 0, 0.7);
  width: 160px;
  height: 100px;
  position: absolute;
  color: #fff;
  padding-top: 15px;
  border-radius: 15px;
  padding-left: 15px;
  box-sizing: border-box;
  left: 0;
  top: 0;
  transition: all 0.3s;
  z-index: 999;
  text-align: left;
  font-size: 12px;
}
</style>