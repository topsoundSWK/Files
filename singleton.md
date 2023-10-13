/** 单例模板 **/
template<class Signle> //模板类型***Signle***
class Signletonbase {

public:
    virtual ~Signletonbase() = default; 

    static std::shared_ptr<Signle> getInstance(); ***Signle的单例获取接口***

protected:
    Signletonbase() = default; 

private:
    static std::mutex mt; //保证线程安全 **静态变量**
    static std::shared_ptr<Signle> m_instance;  ***实例指针***

    Signletonbase(const Signletonbase &my) = delete;//*禁止拷贝构造函数*
    Signletonbase &operator=(const Signletonbase &my) = delete; //*禁止赋值操作*


};

template<class Signle>
std::shared_ptr<Signle> Signletonbase<Signle>::m_instance(nullptr);

template<class Signle>
std::mutex Signletonbase<Signle>::mt;

template<class Signle>
std::shared_ptr<Signle> Signletonbase<Signle>::getInstance() {
    if (m_instance == nullptr) {
        mt.lock();
        if (m_instance == nullptr) {
            m_instance = std::shared_ptr<Signle>(new Signle());
        }
        mt.unlock();
    }
    return m_instance;
}