#### Boost 解析 xml

```cpp
#include <boost/foreach.hpp>
#include <boost/property_tree/ptree.hpp>
#include <boost/property_tree/xml_parser.hpp>
#include <iostream>
#include <set>

using boost::property_tree::ptree;

struct debug_settings {
  std::string m_file;
  int m_level;
  std::set<std::string> m_modules;

  void load(const std::string &filename);
  void save(const std::string &filename);
};

void debug_settings::load(const std::string &filename) {
  using boost::property_tree::ptree;
  ptree pt;

  read_xml(filename, pt);

  m_file = pt.get<std::string>("debug.filename");
  // std::cout << "m_file " << m_file << std::endl;

  m_level = pt.get("debug.level", 0);
  // std::cout << "m_level " << m_level << std::endl;

  BOOST_FOREACH (ptree::value_type &v, pt.get_child("debug.modules")) {
    // v.first 标签名称
    // v.second 标签值
    m_modules.insert(v.second.data());
  }
}

void debug_settings::save(const std::string &filename) {
  using boost::property_tree::ptree;
  ptree pt;

  pt.put("debug.filename", m_file);
  pt.put("debug.level", m_level);

  BOOST_FOREACH (const std::string &name, m_modules) {
    pt.add("debug.modules.module", name);
  }
  write_xml(filename, pt);
}

int main() {
  debug_settings ds;
  ds.load("data.xml");
  ds.save("data_out.xml");
}
```



##### 简单用法

```cpp
#include <boost/foreach.hpp>
#include <boost/property_tree/ptree.hpp>
#include <boost/property_tree/xml_parser.hpp>
#include <iostream>
#include <set>

int main() {
  
  using boost::property_tree::ptree;
  ptree pt;
	
  pt.put("root.name", "stefanlei");
  pt.put("root.age", 18);
  write_xml("out.xml", pt);
}
```

