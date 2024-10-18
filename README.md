import { useEffect, useState } from 'react';
import { Inquiry } from 'backend/generated-rest-client';
import { InquiryList } from 'components/parts/PostList/PostList';
import { InquiryService } from 'services/InquiryService';
import styles from './Home.module.css';

export const HomePage = () => {

  const [inquiries, setInquiries] = useState<Inquiry[]>([]);
  const [sortAscend, setSortAscend] = useState(false);

  useEffect(() => {
   
    const loadInquiries = async () => {
      const inquiries = await InquiryService.getInquiries();
      setInquiries(inquiries);
    };
    loadInquiries();
  }, []);

  useEffect(() => {
  
    const sortedInquiries = [...inquiries].sort((first, second) => {
      if (sortAscend) {
        return new Date(first.datetime).getTime() - new Date(second.datetime).getTime();
      } else {
        return new Date(second.datetime).getTime() - new Date(first.datetime).getTime();
      }
    });
    setInquiries(sortedInquiries);
  }, [sortAscend]);

  let inquiryCount; 
  if (inquiries.length > 9999) {
    // 問



