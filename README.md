MyTest
====================

平时的一些测试小用例
---------------------
### ListView 中带pading的分割线
    在drawable下新建 listview_divider_inset.xml
    <?xml version="1.0" encoding="utf-8"?>
    <inset xmlns:android="http://schemas.android.com/apk/res/android" 
    android:insetLeft="15dp"
    android:insetRight="15dp"
    android:drawable="@color/listview_divider">
    </inset>
    
    在ListView中引用即可:
    <ListView 
    android:id="@+id/lv_test"
    android:layout_height="wrap_content"
    android:layout_width="match_parent"
    android:layout_below="@id/titlebar"
    android:scrollbars="none"
    android:divider="@drawable/listview_divider_inset"
    android:dividerHeight="0.35dp"
    />
    
    效果如下：
![github](https://github.com/handezhao/MyTest/raw/master/picture/divider.png)

### 将adapter中的点击事件移至Activity/Fragment中
        某些场合下我们试图把，adapter中的一些点击事件移至Activity/Fragment中
        看看怎么实现，我的思路是
        在Adapter中定义一个回调接口，在Activity中实现该接口，从而实现对点击事件的响应。
        package com.example.adapter;

import java.util.ArrayList;
import java.util.List;

import android.content.Context;
import android.view.LayoutInflater;
import android.view.View;
import android.view.View.OnClickListener;
import android.view.ViewGroup;
import android.widget.BaseAdapter;
import android.widget.Button;
import android.widget.TextView;

import com.example.bean.TestBean;
import com.example.mytest.R;

public class TestAdapter extends BaseAdapter {
	
	private List<TestBean> list = new ArrayList<TestBean>();
	private Context context;
	private LayoutInflater inflater;
	
	public TestAdapter(ArrayList<TestBean> list, Context context) {
		this.context = context;
		this.list = list;
		this.inflater = LayoutInflater.from(context);
	}
	
	private ButtonListener buttonListener;
	
	public void setButtonListener(ButtonListener listener) {
		this.buttonListener = listener;
	}

	@Override
	public int getCount() {
		
		return list.size();
	}

	@Override
	public Object getItem(int position) {
		return list.get(position);
	}

	@Override
	public long getItemId(int position) {
		return position;
	}

	@Override
	public View getView(final int position, View convertView, ViewGroup parent) {
		ViewHolder holder;
		if (convertView == null) {
			convertView = inflater.inflate(R.layout.item_test_bean, null);
			holder = new ViewHolder();
			holder.button = (Button) convertView.findViewById(R.id.button_item);
			holder.tvInfo = (TextView) convertView.findViewById(R.id.tv_info);
			holder.tvName = (TextView) convertView.findViewById(R.id.tv_name);
			convertView.setTag(holder);
		} else {
			holder = (ViewHolder) convertView.getTag();
		}
		
		holder.button.setText("try");
		holder.tvInfo.setText(list.get(position).getInfo());
		holder.tvName.setText(list.get(position).getName());
		holder.button.setOnClickListener(new OnClickListener() {
			
			@Override
			public void onClick(View view) {
				if (buttonListener != null) {
					buttonListener.onclick(position);
				}
			}
		});
		
		return convertView;
	}
	
	class ViewHolder {
		TextView tvName, tvInfo;
		Button button;
	}
	
	public interface ButtonListener {
		void onclick(int position);
	}
}
        
        在fragment中实现就达到目的了
        adapter.setButtonListener(new ButtonListener() {
				@Override
				public void onclick(int position) {
					Console.toast(getActivity(), String.valueOf(position));
				}
			});
			
###
